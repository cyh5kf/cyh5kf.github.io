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

```
@keyframes	规定动画。	
animation	所有动画属性的简写属性，除了 animation-play-state 属性。	
animation-name	规定 @keyframes 动画的名称。	
animation-duration	规定动画完成一个周期所花费的秒或毫秒。默认是 0。	
animation-timing-function	规定动画的速度曲线。默认是 "ease"。
animation-delay	规定动画何时开始。默认是 0
animation-iteration-count	规定动画被播放的次数。默认是 1。
animation-direction	规定动画是否在下一周期逆向地播放。默认是 "normal"。
animation-play-state	规定动画是否正在运行或暂停。默认是 "running"。

translate3d(x,y,z) translateX(x)	translateY(y)	translateZ(z)	
3d比2d多了z轴的转化

```

* css3有哪些属性

* css3选择器

```
X[title] 属性选择器

X[title=””] 

:first-child

:last-child

:nth-child() 选择器匹配父元素中的第n个子元素

nth-child(odd) nth-child(even)

:nth-last-child(n) 选择器匹配同类型中的倒数第n个同级兄弟元素

:nth-of-type(n) 选择器匹配同类型中的第n个同级兄弟元素。

:only-child :only-child选择器匹配属于父元素中唯一子元素的元素。

:not 选择器匹配每个元素是不是指定的元素/选择器。
```

## 移动端页面

* 移动端H5页面性能优化（条理要清晰）
![移动端性能优化](http://ouq0pnc4r.bkt.clouddn.com/%E7%A7%BB%E5%8A%A8H5%E5%89%8D%E7%AB%AF%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96%E6%8C%87%E5%8D%97.png)

```
概述
PC优化手段在Mobile侧同样适用
在Mobile侧我们提出三秒种渲染完成首屏指标
基于第二点，首屏加载3秒完成或使用Loading
基于联通3G网络平均338KB/s(2.71Mb/s），所以首屏资源不应超过1014KB
Mobile侧因手机配置原因，除加载外渲染速度也是优化重点
基于第五点，要合理处理代码减少渲染损耗
基于第二、第五点，所有影响首屏加载和渲染的代码应在处理逻辑中后置
加载完成后用户交互使用时也需注意性能
加载优化
加载过程是最为耗时的过程，可能会占到总耗时的80%时间，因此是优化的重点

减少HTTP请求
因为手机浏览器同时响应请求为4个请求（Android支持4个，iOS 5后可支持6个），所以要尽量减少页面的请求数，首次加载同时请求数不能超过4个。

a）合并CSS、JavaScript

b）合并小图片，使用雪碧图

缓存
使用缓存可以减少向服务器的请求数，节省加载时间，所以所有静态资源都要在服务器端设置缓存，并且尽量使用长Cache（长Cache资源的更新可使用时间戳）

a） 缓存一切可缓存的资源

b） 使用长Cache（使用时间戳更新Cache）

c） 使用外联式引用CSS、JavaScript

压缩HTML、CSS、JavaScript
减少资源大小可以加快网页显示速度，所以要对HTML、CSS、JavaScript等进行代码压缩，并在服务器端设置GZip。

a） 压缩（例如，多余的空格、换行符和缩进）

b） 启用GZip

无阻塞
写在HTML头部的JavaScript（无异步），和写在HTML标签中的Style会阻塞页面的渲染，因此CSS放在页面头部并使用Link方式引入，避免在HTML标签中写Style，JavaScript放在页面尾部或使用异步方式加载。

使用首屏加载
首屏的快速显示，可以大大提升用户对页面速度的感知，因此应尽量针对首屏的快速显示做优化。

按需加载
将不影响首屏的资源和当前屏幕资源不用的资源放到用户需要时才加载，可以大大提升重要资源的显示速度和降低总体流量。

PS：按需加载会导致大量重绘，影响渲染性能

a） LazyLoad

b） 滚屏加载

c） 通过Media Query加载

预加载
大型重资源页面（如游戏）可使用增加Loading的方法，资源加载完成后再显示页面。但Loading时间过长，会造成用户流失。

对用户行为分析，可以在当前页加载下一页资源，提升速度。

a） 可感知Loading(如进入空间游戏的Loading）

b） 不可感知的Loading（如提前加载下一页）

压缩图片
图片是最占流量的资源，因此尽量避免使用他，使用时选择最合适的格式（实现需求的前提下，以大小判断），合适的大小，然后使用智图压缩，同时在代码中用Srcset来按需显示。

PS：过度压缩图片大小影响图片显示效果

a） 使用智图（ http://zhitu.tencent.com/ ）

b） 使用其它方式代替图片(1. 使用CSS3 2. 使用SVG 3. 使用IconFont）

c） 使用Srcset

d） 选择合适的图片(1. webP优于JPG 2. PNG8优于GIF）

e） 选择合适的大小（1. 首次加载不大于1014KB 2. 不宽于640（基于手机屏幕一般宽度））

延伸阅读：《浓缩的精华！从零开始带你认识最新的图片格式WebP》

减少Cookie
Cookie会影响加载速度，所以静态资源域名不使用Cookie。

避免重定向
重定向会影响加载速度，所以在服务器正确设置避免重定向。

异步加载第三方资源
第三方资源不可控会影响页面的加载和显示，因此要异步加载第三方资源。

脚本执行优化
脚本处理不当会阻塞页面加载、渲染，因此在使用时需当注意：

CSS写在头部，JavaScript写在尾部或异步。

避免图片和iFrame等的空Src，空Src会重新加载当前页面，影响速度和效率。

尽量避免重设图片大小。

重设图片大小是指在页面、CSS、JavaScript等中多次重置图片大小，多次重设图片大小会引发图片的多次重绘，影响性能。

图片尽量避免使用DataURL，DataURL图片没有使用图片的压缩算法文件会变大，并且要解码后再渲染，加载慢耗时长

CSS优化
尽量避免写在HTML标签中写Style属性

避免CSS表达式
CSS表达式的执行需跳出CSS树的渲染，因此请避免CSS表达式。

移除空的CSS规则
空的CSS规则增加了CSS文件的大小，且影响CSS树的执行，所以需移除空的CSS规则。

正确使用Display的属性
Display属性会影响页面的渲染，因此请合理使用。

a） display:inline后不应该再使用width、height、margin、padding以及float

b） display:inline-block后不应该再使用float

c） display:block后不应该再使用vertical-align

d） display:table-*后不应该再使用margin或者float

不滥用Float
Float在渲染时计算量比较大，尽量减少使用。

不滥用Web字体
Web字体需要下载，解析，重绘当前页面，尽量减少使用。

不声明过多的Font-size
过多的Font-size引发CSS树的效率。

值为0时不需要任何单位
为了浏览器的兼容性和性能，值为0时不要带单位。

标准化各种浏览器前缀
a） 无前缀应放在最后

b） CSS动画只用 （-webkit- 无前缀）两种即可

c） 其它前缀为 -webkit- -moz- -ms- 无前缀 四种，（-o-Opera浏览器改用blink内核，所以淘汰）

 避免让选择符看起来像正则表达式
高级选择器执行耗时长且不易读懂，避免使用。

JavaScript执行优化
减少重绘和回流
a） 避免不必要的Dom操作

b） 尽量改变Class而不是Style，使用classList代替className

c） 避免使用document.write

d） 减少drawImage

缓存Dom选择与计算
每次Dom选择都要计算，缓存他。

缓存列表.length
每次.length都要计算，用一个变量保存这个值

尽量使用事件代理，避免批量绑定事件
尽量使用ID选择器，ID选择器是最快的。

TOUCH事件优化
使用touchstart、touchend代替click，因快影响速度快。但应注意Touch响应过快，易引发误操作

渲染优化
HTML使用Viewport
Viewport可以加速页面的渲染，请使用以下代码：

 <meta name=”viewport” content=”width=device-width, initial-scale=1″>
减少Dom节点
Dom节点太多影响页面的渲染，应尽量减少Dom节点

动画优化
a） 尽量使用CSS3动画

b） 合理使用requestAnimationFrame动画代替setTimeout

c） 适当使用Canvas动画 5个元素以内使用css动画，5个以上使用Canvas动画（iOS8可使用webGL）

高频事件优化
Touchmove、Scroll 事件可导致多次渲染

a） 使用requestAnimationFrame监听帧变化，使得在正确的时间进行渲染

b） 增加响应变化的时间间隔，减少重绘次数

GPU加速
CSS中以下属性（CSS3 transitions、CSS3 3D transforms、Opacity、Canvas、WebGL、Video）来触发GPU渲染，请合理使用。

PS：过渡使用会引发手机过耗电增加。
```

* 移动端页面碰到哪些坑，如何解决移动端300ms延迟，多种方法

```
移动端浏览器会有一些默认的行为，比如双击缩放、双击滚动。这些行为，尤其是双击缩放，主要是为桌面网站在移动端的浏览体验设计的。而在用户对页面进行操作的时候，移动端浏览器会优先判断用户是否要触发默认的行为。
1.禁用缩放 <meta name="viewport" content="user-scalable=no">

2.更改默认的视口宽度 <meta name="viewport" content="width=device-width"> 它没有完全禁用缩放，而只是禁用了浏览器默认的双击缩放行为，但用户仍然可以通过双指缩放操作来缩放页面。

3.FastClick FastClick的实现原理是在检测到touchend事件的时候，会通过DOM自定义事件立即出发模拟一个click事件，并把浏览器在300ms之后的click事件阻止掉。
```

* 移动端页面如何提高首屏渲染速度

* 页面滚动卡顿问题查找，问题出在哪里
-webkit-overflow-scrolling: touch;，是因为这行代码启用了硬件加速特性，所以滑动很流畅。

* 移动端页面和pc页面有什么不同

```
1、PC端在开发过程中考虑的是浏览器兼容性，移动端开发中考虑的是手机兼容性问题，做移动端开发，更多考虑的是手机分辨率的自适应和不同手机操作系统的略微差异化；
2、在部分事件的处理上，移动端自然是偏向于触屏的，另外包括移动端弹出的手机键盘该如何处理，这样的问题在PC上肯定不会遇到，但在移动端，如果你没有经验，处理起来是相当麻烦的；
3、布局上，移动端开发是要做到页面布局自适应的，而PC端页面布局的比例会相对固定；
4/手机上的300ms的延迟，这在PC端是没有的
```

* 响应式开发和自适应设计的区别

```
自适应的目的是在不同分辨率的不同设备上面显示相同的页面。目的
响应式的概念应该是覆盖了自适应，但是包括的东西更多了。响应式布局可以根据屏幕的大小自动的调整页面的展现方式，以及布局。是方法
```

* 自适应设计用什么方案，rem单位的理解，rem和em的区别，除了rem之外还有什么方案

* 移动端有哪些触摸事件

```
touchstart：手指触摸到屏幕会触发
touchmove：当手指在屏幕上移动时，会触发
touchend：当手指离开屏幕时，会触发
```

* h5分享功能，调用app分享功能以及微信分享

* h5页面弹出键盘对布局有什么影响，如何处理

js给body一个定高。

设置html和body高度，之后将按钮相对于body定位 (这里的body需要设置高度为100%)：

* h5页面与native如何交互，native如何传递信息给h5，有哪几种方法

```
JSBridge H5->通过某种方式触发一个url->Native捕获到url,进行分析->原生做处理->Native调用H5的JSBridge对象传递回调。
```

## js篇

* 闭包原理，以及闭包的应用

```
作用: 一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中，不会在f1调用后被自动清除
例子: 函数A里的变量a被函数B引用,当函数A被调用完成后,按常理,函数A的上下文环境就出栈被销毁掉,但是因为函数B里面引用到了函数A内的变量a(即函数B依赖于函数A中的变量a),且函数B未被执行完,所以函数A的上下文还没有被销毁,当函数B也执行完后,那么此时函数A和B的上下文都被释放掉(即出栈销毁).

1. 函数作为返回值
function fn1(){
    var max = 10;
    return function (x) {
        if(x > max){
            console.log(x);
        }
    }
}
var fn2 = fn1();
fn2(20);

2. 函数作为参数

var max = 10;
function fn(x){
    if(x > max){
        console.log(x);
    }
}
//匿名函数,最后的(fn)表示调用这个匿名函数
(function(f){
    var max = 100;
    f(15);
})(fn);

function func(param) {
    return function() {
        alert(param);
    }
}
var f = func(1)
setTimeout(f, 1000);

```

* 如何判断数组

```
a instanceof Array;

a.constructor==Array

Array.isArray(a)


```

* 数组有哪些方法

```
join()
push()和pop()
shift() 和 unshift()
sort()
reverse()
concat()
slice()
splice()
indexOf()和 lastIndexOf() （ES5新增）
forEach() （ES5新增）
map() （ES5新增）
filter() （ES5新增）
every() （ES5新增）
some() （ES5新增）
reduce()和 reduceRight() （ES5新增）
```

* 如何解决跨域问题，有哪几种方法，以及jsonp实现原理，服务器响应如何触发回调

```
　　（1）document.domain + iframe

　　（2）动态创建script

　　（3）window.name + iframe

　　（4）window.postMessage

　　（5）CORS

　　（6）JSONP

　　（7）nginx代理

https://www.cnblogs.com/xinxingyu/p/6075881.html
```

* 事件流 IE事件流和W3C事件流有什么区别

```
冒泡型事件模型： button->div->body (IE事件流) 

捕获型事件模型： body->div->button (Netscape事件流) 

DOM事件模型： body->div->button->button->div->body (先捕获后冒泡) 

　　3. 阻止事件冒泡和事件委托的方法：
　　　　A：return false。 
　　　　　　在事件的处理中，可以阻止默认事件和冒泡事件。

　　　　B：event.preventDefault()
　　　　　　在事件的处理中，可以阻止默认事件但是允许冒泡事件的发生。

　　　　C：event.stopPropagation().。
　　　　　　在事件的处理中，可以阻止冒泡但是允许默认事件的发生。
```

* jquery的on，click有什么区别

最大的区别即优点是如果动态创建的元素在该选择器选中范围内是能触发回调函数。

* 解释一下事件委托

事件代理是基于浏览器的事件冒泡机制。事件代理：当我们需要对很多元素添加事件的时候，可以通过将事件添加到它们的父节点而将事件委托给父节点来触发处理函数。



* 如何获取当前点击的元素

e.target  $(this)

* $.ajax里面有哪些参数

```
$.ajax({
    type: "get",
    async: false,
    url: "http://w.api.xiaoying.co/webapi2/rest/video/hotvideolist",
    data: {
        "appkey": "30000000",
        "pagesize": "50",
        "pageindex": "0"
    },
    dataType: "jsonp",
    jsonp: "callback",
    jsonpCallback: "hotvideolist",
    success: function(data) {
    
                                
    },
    error: function() {
        console.log('fail');
    }
});
```

* ajax异步调用原理

```
ajax的原理简单来说通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用javascript来操作DOM而更新页面
    var xmlHttp = new XMLHttpRequest();
    
    xmlHttp.open("get","<%=basePath %>/manage/test/ajax",true);
    xmlHttp.send();
   
    xmlHttp.onreadystatechange = function (){
　　　　　var state = xmlHttp.readyState;
        var status = xmlHttp.status;
        if(state == 4 && status == 200){
            var data=xmlHttp.responseText;
            document.getElementById("ID").value = data;                          
        }
    }
}
```

* add(3, 5)  add(3)(5)   写出一个函数，使结果为3+5

```
function add(x,y){
var sum = x;
    if(y){
        return (sum + y);
    }
    else
    {
        var add2 = function(z){
            return (sum + z);
        }
        return add2;
    }
}
var add1 = add(2,3);
console.log(add1);
var add2 = add(2)(3);
console.log(add2);
```

* 写一个函数，数组求最大数

```
1、字符串拼接法

利用toString和join把数组转换为字符串，再和Math的max和min方法分别进行拼接，最后执行eval方法

var maxN = eval("Math.max(" + ary.toString() + ")");
var minN = eval("Math.min(" + ary.toString() + ")");
或者

var maxN = eval("Math.max(" + ary.join() + ")");
var minN = eval("Math.min(" + ary.join() + ")");

2、排序法

先把数组从小到大排序，数组第一个即为最小值，最后一个即为最大值
ary.sort(function(a,b){return a-b;});
var minN = ary[0];
var maxN = ary[ary.length-1];

3、假设法

假设数组第一个为最大（或最小值），和后边进行比较，若后边的值比最大值大（或比最小值小），则替换最大值（或最小值）
var maxN = ary[0];
var minN = ary[0];
for(var i=1;i<ary.length;i++){
  var cur = ary[i];
  cur>maxN ? maxN=cur : null;
  cur<minN ? minN=cur : null;
}

4、Math的max和min方法

使用apply方法使数组可以作为传递的参数
var maxN = Math.max.apply(null,ary);
var minN = Math.min.apply(null,ary);
```

* 写一个冒泡排序的函数 

```
var arr=new Array();
arr.push(8);
arr.push(4);
arr.push(3);
arr.push(1);
arr.push(5);
arr.push(6);
var temp;
for(var i=0; i<arr.length;i++)
{
    for(var j=i+1;j<arr.length;j++)
    {
         if(arr[i]>arr[j])
          {
             temp=arr[i];
             arr[i]=arr[j];
             arr[j]=temp;
           }
     }
}
```

## http协议

* http协议返回304是什么意思，http常用状态

* http缓存原理

* websokect如何与服务器通信

* HTTP/1.1相比HTTP/1.0有哪些改进

* 数据列表删除使用get请求有什么安全隐患，如何解决

## react框架

* react和vue的区别

* react diff算法原理

```
两棵树的完全的 diff 算法是一个时间复杂度为 O(n^3) 的问题但是在前端当中，很少出现跨越层级移动DOM元素的情况，所以React采用了简化的diff算法，只会对virtual dom中同一个层级的元素进行对比，这样算法复杂度就可以达到 O(n)。

O(n^3) 复杂度的问题转换成 O(n) 复杂度的问题。
1.DOM节点跨层级少，只对同一层节点比较，其他忽略，出现跨层移动，只会删除/新增，影响性能，react官方建议不要进行DOM节点跨层级的操作。
2.两个相同组件产生类似的DOM结构，不同的组件产生不同的DOM结构；
3.对于同一层次的一组子节点，它们可以通过唯一的id进行区分。

不同组件的比较
当React在同一个位置遇到不同的组件时，也是简单的销毁第一个组件，而把新创建的组件加上去。这正是应用了第一个假设，不同的组件一般会产生不一样的DOM结构，与其浪费时间去比较它们基本上不会等价的DOM结构，还不如完全创建一个新的组件加上去。

逐层进行节点比较
在React中，两棵DOM树只会对同一层的节点进行比较。若发现节点已不存在，则该节点及其子节点会被完全删除，不会用于进一步的比较。这样，只需要对树进行一次遍历，就能完成整个DOM树的比较。
对于同层节点，若节点本身完全相同（类型相同，属性相同），只是位置不同，则React只需要考虑同层节点的位置变换，不需要进行节点的销毁和重新创建，这就需要用到下面介绍的key属性，但对于不同层的节点，只能销毁和重新创建。

比较两个虚拟DOM节点，可以分为以下三种情况：
1) 节点类型不同； 
当在树中的同一位置前后的节点类型不同，React会直接删除原节点，然后创建并插入新的节点。
注意：删除节点即彻底销毁该节点，也就是说，后续不会查找是否有另外一个节点等同于删除的该节点。如果删除的该节点有子节点，那么子节点也会被删除。这也是diff算法复杂度能降到O(n)的原因。
同理，当树的同一个位置遇到前后不同的组件时，也是销毁原组件，把新的组件加上去。这应用了第一个假设，不同的组件一般会产生不同的DOM结构，与其浪费时间去比较不同的DOM结构，还不如完全创建一个新的组件加上去。
2) 节点类型相同，但是属性不同。
React会对属性进行重设从而实现节点的转换。
3) 节点类型相同且属性相同。
对于同层节点，若节点本身完全相同（类型相同且属性相同），只是位置不同，则React只需要考虑同层节点的位置变换，不需要进行节点的销毁和重新创建，这就需要用到下面介绍的key属性。
对于不同层的节点，即使节点本身完全相同（类型相同且属性相同），也只能销毁和重新创建。

列表节点的比较

可以看到，对于列表节点提供唯一的key属性可以帮助React定位到正确的节点进行比较，从而大幅减少DOM操作次数，提高了性能。
某虚拟DOM树的某一层原有两个节点a和b，页面更新后得到的新DOM树的该层还是只有a和b，只是a和b的位置与原来不同了。如果未提供唯一的标识key，React将认为a和b对应的位置前后的组件类型不同，而选择完全销毁后重新创建；而如果提供了唯一的标识key，React将能够判断出a和b只是位置不同，更新位置即可。
```

* react不设置key会有什么影响

```
key是一个字符串，用来唯一标识同父同层级的兄弟元素。当React作diff时，只要子元素有key属性，便会去原v-dom树中相应位置（当前横向比较的层级）寻找是否有同key元素，比较它们是否完全相同，若是则复用该元素，免去不必要的操作。
但是强烈不推荐用数组index来作为key。
如果数据更新仅仅是数组重新排序或在其中间位置插入新元素，那么视图元素都将重新渲染。
react的key关乎到react的dom-diff算法 react中对于dom的操作是根据生成的data-reactid进行绑定的，添加key可以保证dom结构的完整性，而不会根据react自己对dom标记的key进行重新分配 react每次决定重新渲染的时候，几乎完全是根据data-reactid来决定的，最重要的是这个机制
```

* react父子组件如何通信，两个相隔很远的组件的如何通信

```
父组件向子组件通信：使用 props
子组件向父组件通信：使用 props 回调
跨级组件间通信：使用 context 对象
非嵌套组件间通信：使用事件订阅
redux

https://www.jianshu.com/p/fb915d9c99c4

```

* react的state和props的区别，为什么不能修改props

```
props会在整个组件数中传递数据和配置，props可以设置任命类型的数据，应该把它当做组件的数据源。其不但可以用于上级组件与下组件的通信，也可以用其做为事件处理器。
state只能在组件内部使用，state只应该用于存储简单的视图状（如：上面示例用于控制下拉框的可见状态）。
props和state都不能直接修改，而应该分别使用setProps()和setSate()方法修改。
```

* react操作虚拟DOM比原生DOM操作好在哪里
```
首先, 虚拟dom并没有比直接原生操作更快, 所谓"快"是有条件的
比如点赞, 数字+1, 直接操作dom会更快.
如果你能自己捋请规则, 每回手动操作dom的时候, 都只改动应该改变的, 那dom操作永远比虚拟dom快.
但如果你的改动勾连的地方很多, 而且要保持状态, 那虚拟dom的自动diff无疑会让你省更多心.
比如一个列表, 列表项有点赞等状态, 回复数量等信息, 有动态新增, 有动态加载, 这时候你要直接操作dom会很繁琐.
虚拟dom的核心在于diff, 它自动帮你计算那些应该调整, 然后只修改该修改的区域, 省下的不是运行速度这种小速度, 而是开发速度/维护速度/逻辑简练程度等"总体速度"
```

* react性能优化

```
https://segmentfault.com/a/1190000007811296
https://segmentfault.com/a/1190000010438089

1,React.PureComponent(浅比较)
2，immutable.js
Immutable 实现的原理是 Persistent Data Structure（持久化数据结构），也就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。同时为了避免 deepCopy 把所有节点都复制一遍带来的性能损耗，Immutable 使用了 Structural Sharing（结构共享），即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其它节点则进行共享。
Immutable 则提供了简洁高效的判断数据是否变化的方法，只需 === 和 is 比较就能知道是否需要执行 render()，而这个操作几乎 0 成本，所以可以极大提高性能。修改后的 shouldComponentUpdate 是这样的：
import { is } from 'immutable';

shouldComponentUpdate: (nextProps = {}, nextState = {}) => {
  const thisProps = this.props || {}, thisState = this.state || {};

  if (Object.keys(thisProps).length !== Object.keys(nextProps).length ||
      Object.keys(thisState).length !== Object.keys(nextState).length) {
    return true;
  }

  for (const key in nextProps) {
    if (!is(thisProps[key], nextProps[key])) {
      return true;
    }
  }

  for (const key in nextState) {
    if (thisState[key] !== nextState[key] || !is(thisState[key], nextState[key])) {
      return true;
    }
  }
  return false;
}

react-immutable-render-mixin

无状态组件

高阶组件
```

* react路由如何异步加载
https://www.cnblogs.com/little-ab/articles/6956341.html

```
<Route path="/baidu"
　　　getComponent(nextState, cb) { require.ensure([], (require) => { cb(null, require('components/layer/HomePage')) }, 'HomePage')}>

es6写法
getComponent(nextState, cb) {
  require.ensure([], (require) => {
   // 在后面加 .default
   cb(null, require('components/layer/ReportPage')).default
  }, 'ReportPage')
}

这是 webpack 提供的方法，这也是按需加载的核心方法。第一个参数是依赖，第二个是回调函数，第三个就是上面提到的 chunkName，用来指定这个 chunk file 的 name。
```

* react setState之后组件发生什么变化，render过程

* immutable原理，immutable对象如何创建的，与普通的js对象有什么区别，优点，缺点，如何与shouldComponentUpdate使用

* react生命周期描述，shouldComponentUpdate作用

```
这几个生命周期相关的函数有：
constructor(props, context)
构造函数，在创建组件的时候调用一次。

void componentWillMount()
在组件挂载之前调用一次。如果在这个函数里面调用setState，本次的render函数可以看到更新后的state，并且只渲染一次。

void componentDidMount()
在组件挂载之后调用一次。这个时候，子主键也都挂载好了，可以在这里使用refs。

void componentWillReceiveProps(nextProps)
props是父组件传递给子组件的。父组件发生render的时候子组件就会调用
componentWillReceiveProps（不管props有没有更新，也不管父子组件之间有没有数据交换）。

bool shouldComponentUpdate(nextProps, nextState)
组件挂载之后，每次调用setState后都会调用shouldComponentUpdate判断是否需要重新渲染组件。默认返回true，需要重新render。在比较复杂的应用里，有一些数据的改变并不影响界面展示，可以在这里做判断，优化渲染效率。

void componentWillUpdate(nextProps, nextState)
shouldComponentUpdate返回true或者调用forceUpdate之后，componentWillUpdate会被调用。

void componentDidUpdate()
除了首次render之后调用componentDidMount，其它render结束之后都是调用componentDidUpdate。

componentWillMount、componentDidMount和componentWillUpdate、componentDidUpdate可以对应起来。区别在于，前者只有在挂载的时候会被调用；而后者在以后的每次更新渲染之后都会被调用。

ReactElement render()
render是一个React组件所必不可少的核心函数（上面的其它函数都不是必须的）。记住，不要在render里面修改state。

void componentWillUnmount()
组件被卸载的时候调用。一般在componentDidMount里面注册的事件需要在这里删除。
```

* react pureRender原理
修改shouldComponentUpdate，只是浅比较

* react-router实现原理，如何兼容低版本浏览器，不用html5 history  低版本用hash 
https://segmentfault.com/a/1190000004527878
https://www.cnblogs.com/wyaocn/p/5805777.html

```
createBrowserHistory: 利用HTML5里面的history

createHashHistory: 通过hash来存储在不同状态下的history信息

createMemoryHistory: 在内存中进行历史记录的存储

1.2.1 执行URL前进
createBrowserHistory: pushState、replaceState

createHashHistory: location.hash=*** location.replace()

createMemoryHistory: 在内存中进行历史记录的存储

1.2.2 检测URL回退
createBrowserHistory: popstate

createHashHistory: hashchange

createMemoryHistory: 因为是在内存中操作，跟浏览器没有关系，不涉及UI层面的事情，所以可以直接进行历史信息的回退

```
![移动端性能优化](http://ouq0pnc4r.bkt.clouddn.com/react-router.png)

* redux概述原理，使用redux有什么问题，reducer如何管理

```
import React, {Component} from 'react'

class Counter extends Component {
    render() {
        //从组件的props属性中导入四个方法和一个变量
        const {increment, decrement, counter} = this.props;
        //渲染组件，包括一个数字，四个按钮
        return (
            <p>
                Clicked: {counter} times
                {' '}
                <button onClick={increment}>+</button>
                {' '}
                <button onClick={decrement}>-</button>
                {' '}
            </p>
        )
    }
}

export default Counter;


import { connect } from 'react-redux'
import Counter from '../components/Counter'
import actions from '../actions/counter';

//将state.counter绑定到props的counter
const mapStateToProps = (state) => {
    return {
        counter: state.counter
    }
};
//将action的所有方法绑定到props上
const mapDispatchToProps = (dispatch, ownProps) => {
    return {
        increment: (...args) => dispatch(actions.increment(...args)),
        decrement: (...args) => dispatch(actions.decrement(...args))
    }
};

//通过react-redux提供的connect方法将我们需要的state中的数据和actions中的方法绑定到props上
export default connect(mapStateToProps, mapDispatchToProps)(Counter)


合并多个reducer
// 创建两个reducer: count year
function count (state, action) {
  state = state || {count: 1}
  switch (action.type) {
    default:
      return state;
  }
}
function year (state, action) {
  state = state || {year: 2015}
  switch (action.type) {
    default:
      return state;
  }
}

// 将多个reducer合并成一个
var combineReducers = require('./').combineReducers;
var rootReducer = combineReducers({
  count: count,
  year: year,
});

// 创建store，跟2.1没有任何区别
var createStore = require('./').createStore;
var store = createStore(rootReducer);
```

* redux如何将store的数据更新渲染到view层

* 假设有多个reducer，action是如何找到对应的reducer进行状态的转换？
https://segmentfault.com/q/1010000012086401/a-1020000012111603

```
store根据dispatch之后返回的action对象中的 type 属性来执行相关任务，也就是说只要带有相同 type 属性值的reducer都会执行。
```

* react组件如何实现双向绑定{{}}，不使用setState
https://github.com/lhang/blog/issues/3

* react 服务器端渲染

* react 为什么要将class属性变成className

* bable编译原理

* hash和browser有什么区别,hash和browser页面跳转之后可以定位到之前的路由信息吗？

```
首先 browserHistory 其实使用的是 HTML5 的 History API，浏览器提供相应的接口来修改浏览器的历史记录；而 hashHistory 是通过改变地址后面的 hash 来改变浏览器的历史记录；

History API 提供了 pushState() 和 replaceState() 方法来增加或替换历史记录。而 hash 没有相应的方法，所以并没有替换历史记录的功能。但 react-router 通过 polyfill 实现了此功能，具体实现没有看，好像是使用 sessionStorage。
```

* webpack loder和plugin有什么区别

```
loader 用于加载某些资源文件。 因为webpack 本身只能打包commonjs规范的js文件，对于其他资源例如 css，图片，或者其他的语法集，比如 jsx， coffee，是没有办法加载的。 这就需要对应的loader将资源转化，加载进来。从字面意思也能看出，loader是用于加载的，它作用于一个个文件上。

plugin 用于扩展webpack的功能。它直接作用于 webpack，扩展了它的功能。当然loader也时变相的扩展了 webpack ，但是它只专注于转化文件（transform）这一个领域。而plugin的功能更加的丰富，而不仅局限于资源的加载。
```

* webpack打包原理

```
依赖管理：方便引用第三方模块、让模块更容易复用、避免全局注入导致的冲突、避免重复加载或加载不需要的模块。

合并代码：把各个分散的模块集中打包成大文件，减少HTTP的请求链接数，配合UglifyJS可以减少、优化代码的体积。

各路插件：babel把ES6+转译成ES5-，eslint可以检查编译期的错误……
```

* webpack插件机制

* webpack 如何区分配置开发环境和生产环境, source-map有哪几种, react热更新插件

```
cross-env NODE_ENV=production

eval： 生成代码 每个模块都被eval执行，并且存在@sourceURL

cheap-eval-source-map： 转换代码（行内） 每个模块被eval执行，并且sourcemap作为eval的一个dataurl

cheap-module-eval-source-map： 原始代码（只有行内） 同样道理，但是更高的质量和更低的性能

eval-source-map： 原始代码 同样道理，但是最高的质量和最低的性能

cheap-source-map： 转换代码（行内） 生成的sourcemap没有列映射，从loaders生成的sourcemap没有被使用

cheap-module-source-map： 原始代码（只有行内） 与上面一样除了每行特点的从loader中进行映射

source-map： 原始代码 最好的sourcemap质量有完整的结果，但是会很慢
```

## vuejs

* vue生命周期有哪些
https://www.cnblogs.com/penghuwan/p/7192203.html
![移动端性能优化](http://ouq0pnc4r.bkt.clouddn.com/vue%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.jpg）

```

1. 在beforeCreate和created钩子函数之间的生命周期
在这个生命周期之间，进行初始化事件，进行数据的观测，可以看到在created的时候数据已经和data属性进行绑定（放在data中的属性当值发生改变的同时，视图也会改变）。

2. created钩子函数和beforeMount间的生命周期
首先会判断对象是否有el选项。如果有的话就继续向下编译，如果没有el选项，则停止编译，也就意味着停止了生命周期，直到在该vue实例上调用vm.$mount(el)。
template参数选项的有无对生命周期的影响。
（1）.如果vue实例对象中有template参数选项，则将其作为模板编译成render函数。
（2）.如果没有template选项，则将外部HTML作为模板编译。
（3）.可以看到template中的模板优先级要高于outer HTML的优先级。
render函数选项 > template选项 > outer HTML.

3. beforeMount和mounted 钩子函数间的生命周期
可以看到此时是给vue实例对象添加$el成员，并且替换掉挂在的DOM元素。因为在之前console中打印的结果可以看到beforeMount之前el上还是undefined。

4. mounted
挂载完成

5. beforeUpdate钩子函数和updated钩子函数间的生命周期
当vue发现data中的数据发生了改变，会触发对应组件的重新渲染，先后调用beforeUpdate和updated钩子函数。

6. updated
更新完成

7.beforeDestroy和destroyed钩子函数间的生命周期
beforeDestroy钩子函数在实例销毁之前调用。在这一步，实例仍然完全可用。
destroyed钩子函数在Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
```

* vue父子组件如何互相通信

```
父传子
//方式一
props: ['mdzz'],
//方式二
props: {
    mdzz: String //这里指定了字符串类型，如果类型不一致会警告
},
//方式三
props: {
    mdzz: {
        type: String,
        default: '' 
    }
},

子组件向父组件传递数据用$emit方法，
//parent.vue
template:
<child @showbox="toshow"></child>
js:
methods:{
    toshow(msg) {
       this.msg2 = msg;
    },
}
//child.vue
template:
<input @click="open" type="button" value="按钮" />
js:
methods:{
    open() {
        this.$emit('showbox','我是子组件数据'); 
            //触发showbox方法，'我是子组件数据'为向父组件传递的数据
        }
}

无直接关系组件之间的数据传递
let vm = new Vue(); //创建一个新实例
<div @click="ge"></div>
methods: {
    ge() {
        vm.$emit('blur','哈撒ki'); //触发事件
    }
}

<div></div>
created() {
  vm.$on('blur', (msg) => { 
        this.data = msg; // 接收数据
    });
}
```

* vuex概述

* vue-router概述

```
<router-link to="/goods">商品</router-link>
<router-link to="/ratings">评论</router-link>
<router-link to="/seller">商家</router-link>

// 首先定义或者引入路由的组件
// 方法一：直接定义路由组件
const goods = { template: '<p>goods</p>' };
const ratings = { template: '<p>ratings</p>' };
const seller = { template: '<p>seller</p>' };
// 方法二：import引入路由组件
import goods from 'components/goods/goods';
import ratings from 'components/ratings/ratings';
import seller from 'components/seller/seller';
// 然后定义路由(routes)，components还可以是Vue.extend()创建的
const routes = [
  { path: '/goods', component: goods },
  { path: '/ratings', component: ratings },
  { path: '/seller', component: seller }
];
// 接着创建路由实例
const router = new VueRouter({
  // ES6缩写语法，相当于routes:routes
  routes  
});
// 最后创建vue实例并挂载
const app = new Vue({
  el: '#app',
  router
});
// 或者
const app = new Vue({
  router
}).$mount('#app')

router-link属性配置

replace
一个布尔类型，默认为false。如果replace设置为true，那么导航不会留下history记录，点击浏览器回退按钮不会再回到这个路由。

tag
router-link默认渲染成a标签，也有方法让它渲染成其他标签，tag属性就用来设置router-link渲染成什么标签的。

active-class
上面说了被选中的router-link将自动添加一个class属性值.router-link-active，这个属性就是来修改这个class值的。

router-view
这个组件十分关键，它就是用来渲染匹配到的路由的。 
可以给router-view组件设置transition过渡，具体用法见Vue2.0 Transition常见用法全解惑。 
还可以配合<keep-alive>使用，keep-alive可以缓存数据，这样不至于重新渲染路由组件的时候，之前那个路由组件的数据被清除了。比如对当前的路由组件a进行了一些DOM操作之后，点击进入另一个路由组件b，再回到路由组件a的时候之前的DOM操作还保存在，如果不加keep-alive再回到路由组件a时，之前的DOM操作就没有了，得重新进行。如果你的应用里有一个购物车组件，就需要用到keep-alive。

<transition>
  <keep-alive>
    <router-view></router-view>
  </keep-alive>
</transition>

不同页面之间的标题
// 关键在这里，设置afterEach钩子函数
router.afterEach((to, from, next) => {
  document.title = to.name;
})

重定向
const routes = [
  { path: '/', redirect: '/goods'}
]

导航式编程
// 在创建vue实例并挂载后调用
router.push('/goods')
```

* vue-router钩子

```
router.beforeEach((to, from, next) => {
  // ...
})
每个钩子方法接收三个参数：

    to: Route: 即将要进入的目标 路由对象

    from: Route: 当前导航正要离开的路由

    next: Function: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。

        next(): 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed （确认的）。

        next(false): 中断当前的导航。如果浏览器的 URL 改变了（可能是用户手动或者浏览器后退按钮），那么 URL 地址会重置到 from 路由对应的地址。

        next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。

确保要调用 next 方法，否则钩子就不会被 resolved。

某个路由独享的钩子

const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

Vue2.0 Transition

```
<!-- 首先将要过渡的元素用transition包裹，并设置过渡的name，然后添加触发这个元素过渡的按钮（实际项目中不一定是按钮，任何能触发过渡组件的DOM操作的操作都可以） -->
<div>
  <button @click="show=!show">show</button>
  <transition name="fade">
    <p v-show="show">hello</p>
  </transition>
</div>

// 接着为过渡类名添加规则
&.fade-enter-active, &.fade-leave-active
  transition: all 0.5s ease     
&.fade-enter, &.fade-leave-active
  opacity: 0

fade-enter：进入过渡的开始状态，元素被插入时生效，只应用一帧后立即删除；
fade-enter-active：进入过渡的结束状态，元素被插入时就生效，在过渡过程完成之后移除；
fade-leave：离开过渡的开始状态，元素被删除时触发，只应用一帧后立即删除；
fade-leave-active：离开过渡的结束状态，元素被删除时生效，离开过渡完成之后被删除；

CSS动画
<div>
  <button @click="show=!show">show</button>
  <transition name="fold">
    <p v-show="show">hello</p>
  </transition>
</div>

.fold-enter-active {
  animation-name: fold-in;
  animation-duration: .5s;
}
.fold-leave-active {
  animation-name: fold-out;
  animation-duration: .5s;
}
@keyframes fold-in {
  0% {
    transform: translate3d(0, 100%, 0);
  }
  50% {
    transform: translate3d(0, 50%, 0);
  }
  100% {
    transform: translate3d(0, 0, 0);
  }
}
@keyframes fold-out {
  0% {
    transform: translate3d(0, 0, 0);
  }
  50% {
    transform: translate3d(0, 50%, 0);
  }
  100% {
    transform: translate3d(0, 100%, 0);
  }
}

路由懒加载
const Foo = resolve => {
  // require.ensure 是 Webpack 的特殊语法，用来设置 code-split point
  // （代码分块）
  require.ensure(['./Foo.vue'], () => {
    resolve(require('./Foo.vue'))
  })
}
```

## es6

* es6有哪些新特性，分别描述
https://github.com/laizimo/zimo-article/issues/38

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



