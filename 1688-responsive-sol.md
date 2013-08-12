## 响应式布局并兼容ie6， 7， 8
====

### 需求

1. 响应式页面只存在三种宽度，分别为990px、1190px、1390px,遵循栅格系统30a+10i或40a+10i。其中1190px将成为后续页面设计的基础尺寸。

2. 当页面resize时，三种页面宽度切换的中间渐变状态前端可选择性实现。

3. 对IE6、7、8浏览器，页面的加载尺寸根据当前浏览器大小自动定位到三种宽度之一，之后浏览     器resize时，页面的宽度无需做响应式的变化。


### 解决方案

1. 在这里， 对于响应式布局的需求更多的是对宽度的mediaQuery， 也就是对min-width ， max-width 的设置， 而不需要对media type这些复杂的查询条件。 对于ie6, 7, 8一个比较通用的响应解决方案就是通过ajax方式加载css， 并对responseText进行处理， 处理的parser很简单， 只需要正则匹配出media query的组， 并找出minWidth 和maxWidth。

2. parser处理过后将所有的mediaQuery组缓存起来， 放到内存当中， 下次加载的时候直接通过内存加载，  （如果是通过cdn方式获取的css， 需要解决一下ajax跨域问题。）  最后根据mediaQuery将合适的css通过style.cssText方式append到Dom当中。  

3. 监听window 的 resize事件， 事件处理函数判断当前的浏览器宽度有变化， 那么在dom中remove掉之前加到dom中的style， 并重之前的缓存中找到合适的css再次append到dom中。

4. 对于至此media Query的浏览器来说，更具url直接设置link就可以。

5. 对于中间渐变过程， 对于支持css3的浏览器toggole一个有用transition-duration的类就行。  而对于ie6， 7， 8。要实现渐变，
   可以在js中通过kissy Anim模块对较大的div块进行Anim。 

6. 对于ie6， 7， 8的blink 需要在上述脚本执行前进行图片缓存设置及其他一些处理。

7. 可以参考[responsive.js](https://github.com/scottjehl/Respond)的代码实现。

