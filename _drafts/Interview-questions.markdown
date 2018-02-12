# web前端面试题集合

## html，css篇

* meta viewport原理，属性

```
width

控制 viewport 的大小，可以指定的一个值，如果 600，或者特殊的值，如 device-width 为设备的宽度（单位为缩放为 100% 时的 CSS 的像素）。


height

和 width 相对应，指定高度。


initial-scale

初始缩放比例，也即是当页面第一次 load 的时候缩放比例，为一个数字，可以带小数。


maximum-scale

允许用户缩放到的最大比例，为一个数字，可以带小数。


minimum-scale

允许用户缩放到的最小比例，为一个数字，可以带小数。


user-scalable

用户是否可以手动缩放。
```

* 如何实现手机端自适应，rem单位的理解

```
1.网易的做法
如果设计稿基于iphone6，横向分辨率为750，为了计算方便，取一个100px的font-size为参照，body的width为750 / 100 = 7.5rem
html的font-size计算公式为：document.documentElement.style.fontSize = document.documentElement.clientWidth / 7.5 + 'px';
如果一个元素的设计稿高度为210px，那么height: 2.1rem;
font-size可能需要额外的媒介查询，并且font-size不能使用rem
@media screen and (max-width:321px){
    .m-navlist{font-size:15px}
}

@media screen and (min-width:321px) and (max-width:400px){
    .m-navlist{font-size:16px}
}

@media screen and (min-width:400px){
    .m-navlist{font-size:18px}
}
同时视口要设置为：
<meta name="viewport" content="initial-scale=1,maximum-scale=1, minimum-scale=1">
另外还要设置一个最大值，当deviceWidth大于设计稿的横向分辨率时，html的font-size始终等于横向分辨率/body元素宽：
var deviceWidth = document.documentElement.clientWidth;
if(deviceWidth > 640) deviceWidth = 640;
document.documentElement.style.fontSize = deviceWidth / 7.5 + 'px';

2.淘宝的做法
（1）动态设置viewport的scale
var scale = 1 / devicePixelRatio;
document.querySelector('meta[name="viewport"]').setAttribute('content','initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no');
（2）动态计算html的font-size
document.documentElement.style.fontSize = document.documentElement.clientWidth / 10 + 'px';
（3）布局的时候，各元素的css尺寸=设计稿标注尺寸/设计稿横向分辨率/10
（4）font-size可能需要额外的媒介查询，并且font-size不使用rem，这一点跟网易是一样的。

rem：是一个相对单位，相对根元素字体大小的单位，再直白点就是相对于html元素字体大小的单位。

```

* css盒子模型，boder-sizing:border-box  float浮动

```
css盒子模型 又称框模型 (Box Model) ，包含了元素内容（content）、内边距（padding）、边框（border）、外边距（margin）几个要素

box-sizing:content   总宽度=margin+border+padding+width

box-sizing:border-box    总宽度=margin+width

关于border-box的使用：
1 一个box宽度为100%，又想要两边有内间距，这时候用就比较好
2 全局设置 border-box 很好，首先它符合直觉，其次它可以省去一次又一次的加加减减，它还有一个关键作用——让有边框的盒子正常使用百分比宽度。
```

* 弹出窗口居中方法,列出几种方法。

```
1.  width: 100%;
    height: 150px;
    position: absolute;
    left: 50%;
    top: 50%;
    margin-left: -50%;
    margin-top: -75px;

2.  width: 250px;
    height: 150px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);

3. 父元素display: table;子元素display: table-cell;
    元素内容width: 50%;
    height: 50%;
    margin: auto;

4. 父元素text-align: center;子元素display: inline-block;vertical-align: middle;

5. 伸缩盒子模型
    父元素
    display: flex;
    align-items: center;
    justify-content: center;

6. 绝对居中块
    父元素
    width: 500px;
    height: 300px;
    position: absolute;

    子元素
    width: 50%;
    height: 50%;
    margin: auto;
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
```

* 浏览器兼容性问题有哪些？

```
1.块属性标签float后，又有横行的margin情况下，在IE6显示margin比设置的大（即双倍边距bug）

问题症状：常见症状是IE6中后面的一块被顶到下一行

解决方案：在float的标签样式控制中加入 display:inline;将其转化为行内属性

2.图片默认有间距

问题症状：几个img标签放在一起的时候，有些浏览器会有默认的间距，加了问题一中提到的通配符也不起作用。

解决方案：使用float属性为img布局

3.min-height最小高度的实现 {min-height:200px; height:auto !important; height:200px; overflow:visible;}

```

* css3有哪些属性

```
border-radius:25px;
box-shadow: 10px 10px 5px #888888;
border-image:url(border.png) 30 30 round;
background-size:63px 100px;
background-origin:content-box; 背景图片的定位区域
text-shadow: 5px 5px 5px #FF0000; 文本阴影
word-wrap:break-word;  允许对长单词进行拆分，并换行到下一行
transform: translate(50px,100px); 位移
transform: rotate(30deg); 旋转
transform: scale(2,4);  缩放
transform: skew(30deg,20deg);  翻转
transform: rotateX(120deg);
transform: rotate3d(x,y,z);
transition: width 1s linear 2s;  过渡
animation: myfirst 5s;
@keyframes myfirst
{
from {background: red;}
to {background: yellow;}
}
@keyframes myfirst
{
0%   {background: red;}
25%  {background: yellow;}
50%  {background: blue;}
100% {background: green;}
}

box-sizing:border-box;
```

* css两栏布局，已知左右宽度，高度随内容变化，如何实现左右两块等高

* css3动画，有哪些属性   traslate 3d跟2d有什么区别

* css3有哪些属性

* css3选择器

## 移动端页面

* 移动端H5页面性能优化（条理要清晰）

* 移动端页面碰到哪些坑，如何解决移动端300ms延迟，多种方法

* 移动端页面如何提高首屏渲染速度

* 页面滚动卡顿问题查找，问题出在哪里

* 移动端页面和pc页面有什么不同

* 响应式开发和自适应设计的区别

* 自适应设计用什么方案，rem单位的理解，rem和em的区别，除了rem之外还有什么方案

* 移动端有哪些触摸事件

* h5分享功能，调用app分享功能以及微信分享

* h5页面弹出键盘对布局有什么影响，如何处理

* h5页面与native如何交互，native如何传递信息给h5，有哪几种方法

## js篇

* 闭包原理，以及闭包的应用

* 如何判断数组

* 数组有哪些方法

* 如何解决跨域问题，有哪几种方法，以及jsonp实现原理，服务器响应如何触发回调

* 事件流 IE事件流和W3C事件流有什么区别

* jquery的on，click有什么区别

* 解释一下事件委托

* 如何获取当前点击的元素

* $.ajax里面有哪些参数

* ajax异步调用原理

* add(3, 5)  add(3)(5)   写出一个函数，使结果为3+5

* 写一个函数，数组求最大数

* 写一个冒泡排序的函数 

## http协议

* http协议返回304是什么意思，http常用状态

* http缓存原理

* websokect如何与服务器通信

* HTTP/1.1相比HTTP/1.0有哪些改进

* 数据列表删除使用get请求有什么安全隐患，如何解决

## react框架

* react和vue的区别

* react diff算法原理

* react不设置key会有什么影响

* react父子组件如何通信，两个相隔很远的组件的如何通信

* react的state和props的区别，为什么不能修改props

* react操作虚拟DOM比原生DOM操作好在哪里

* react性能优化

* react路由如何异步加载

* react setState之后组件发生什么变化，render过程

* immutable原理，immutable对象如何创建的，与普通的js对象有什么区别，优点，缺点，如何与shouldComponentUpdate使用

* react生命周期描述，shouldComponentUpdate作用

* react pureRender原理

* react-router实现原理，如何兼容低版本浏览器，不用html5 history  低版本用hash 

* redux概述原理，使用redux有什么问题，reducer如何管理

* redux如何将store的数据更新渲染到view层

* 假设有多个reducer，action是如何找到对应的reducer进行状态的转换？

* react组件如何实现双向绑定{{}}，不使用setState

* react 服务器端渲染

* react 为什么要将class属性变成className

* bable编译原理

* hash和browser有什么区别,hash和browser页面跳转之后可以定位到之前的路由信息吗？

* webpack loder和plugin有什么区别

* webpack打包原理

* webpack插件机制

* webpack 如何区分配置开发环境和生产环境, source-map有哪几种, react热更新插件

* webpack2和1的区别

## vuejs

* vue生命周期有哪些

* vue父子组件如何互相通信

* vuex概述

* vue-router概述

* vue-router钩子

## es6

* es6有哪些新特性，分别描述

* const声明一个对象可以改变值吗

* let，const用es5是如何实现的

* promise如何实现即时发生错误仍然执行一段代码

* promise的两种状态

* 数组去重，用es6如何实现

* 箭头函数和function函数区别

* this的指向问题

* 分别用es6和原生js实现继承

## nodejs

* express和koa有什么区别？

* node单线程，如何利用服务器cpu多核

* nginx反向代理和代理的区别

* commonjs规范中的require实现原理



