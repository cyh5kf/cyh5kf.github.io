---
layout: post
title: 前端跳槽面试必备技巧
date: 2017-08-30 12:00:00.000000000 +08:00
---

## 面试准备

### 职位描述分析
#### JD
#### 职位描述，工作职责
* PC端和移动端都需要掌握
* 负责APP H5开发，涉及Hybrid，纯H5开发，动画，3D
* 与后端工程师沟通，调试数据接口
* 前端组件库，这个要求比较高
* 系统优化重构

#### 任职要求，技术要求，技术能力
* HTML5最新规范特性
* 熟悉js类库，js面向对象
* web标准，前后端数据分离，HTML语义化，熟悉框架，有实际经验
* 前端架构分析与设计能力
* 用户体验，用户可用性，用户研究
* 对web前端技术有强烈兴趣，看github前端项目，看技术博客
* css预编译，less，sass
* 熟悉web构建工具，grunt, gulp, webpack, 能够自己搭建前端构建环境，grunt和gulp的区别
* 有服务器端开发经验者优先，除非自己擅长的，否则不要写

先判断这个岗位是否是自己喜欢的，而是判断这个岗位的要求自己是否能hold住

### 艺龙职位要求
#### 职位描述
* 负责前端开发，系统化设计，模块化设计，前后端分离
* 负责运营推广的H5，PC移动端都有可能，采用canvas、css3、JS相关前端技术
* 负责艺龙微信项目，微信小程序，微信支付，微信开发的坑
* 前端框架的开发与维护

#### 岗位要求
* web标准，es6标准，可用性和可访问性，性能要求，异常监控，资源加载错误，JS运行错误
* 熟练使用工程化工具，熟悉webpack,grunt,gulp,sass
* 具有良好代码风格
* 熟悉后端语言，nodejs，有实践经验
* 个性活泼开朗，逻辑性强

### 业务分析或实战模拟（JD）
查看该公司网站，组件，准备如何实现方案

在控制台查看网站的细节，源码分析，使用了哪些框架工具

双核浏览器优先使用webkit内核，`<meta name="renderer" content="webkit">`

DNS预解析 `<link dns-prefetch href="//static.360buyimg.com/"`

localstorage

字体文件，自定义字体，字体图标

webp图片格式

### 技术栈准备

### 自我介绍

## 一面
### 页面布局
题目：假设高度已知，请写出三栏布局，其中左栏、右栏宽度各300px,中间自适应
	
```
<head>
	<style>
		* {
			padding: 0;
			margin: 0;
		}
		.layout {
			margin-top: 20px;
		}
		.layout article div {
			min-height: 100px;
		}
	</style>
</head>
<body>
	<section class="layout float">
		<style>
			.layout.float .left {
				float: left;
				width: 300px;
				background: red;
			}
			.layout.float .right {
				float: rigth;
				width: 300px;
				background: blue;
			}
			.layout.float .center {
				background: yellow;
			}
		</style>
		<article class="left-right-center>
			<div class="left"></div>
			<div class="center">
				<h1>浮动解决方案</h1>
			</div>
			<div class="right"></div>
		</article>
	</section>
	
	<section class="layout absolute">
		<style>
			.layout.absolute .left-right-center div {
				position: absolute;
			}
			.layout.absolute .left {
				left: 0;
				width: 300px;
				background: red;
			}
			.layout.absolute .center {
				left: 300px;
				right: 300px;
				background: blue;
			}
			.layout.absolute . right {
				right: 0;
				width: 300px;
				background: yellow;
			}
		</style>
		<article class="left-right-center>
			<div class="left"></div>
			<div class="center">
				<h1>绝对定位解决方案</h1>
			</div>
			<div class="right"></div>
		</article>
	</section>
	
	<section class="layout flexbox">
		<style>
			.layout.flexbox {
				margin-top:110px;
			}
			.layout.flexbox .left-right-center {
				display: flex;
			}
			.layout.flexbox .left {
				width: 300px;
				background: red;
			}
			.layout.flexbox .right {
				width: 300px;
				background: blue;
			}
			.layout.flexbox .center {
				flex: 1;
				background: yellow;
			}
		</style>
		<article class="left-right-center>
			<div class="left"></div>
			<div class="center">
				<h1>flexbox解决方案</h1>
			</div>
			<div class="right"></div>
		</article>
	</section>
	
	<section class="layout table">
		<style>
			.layout.table .left-right-center {
				width: 100%;
				display: table;
				height: 100px;
			}
			
			.layout.table .left-right-center div {
				display: table-cell;
			}
			.layout.table .left {
				width: 300px;
				background: red;
			}
			.layout.table .center {
				background: blue;
			}
			.layout.table .right {
				width: 300px;
				background: yellow;
			}
		</style>
		<article class="left-right-center>
			<div class="left"></div>
			<div class="center">
				<h1>table解决方案</h1>
			</div>
			<div class="right"></div>
		</article>
	</section>
	
	<section class="layout grid">
		<style>
			.layout.grid .left-right-center {
				width: 100%;
				display: grid;
				grid-template-rows: 100px;
				grid-template-columns: 300px auto 300px;
			}
		
			.layout.grid .left {
				background: red;
			}
			.layout.grid .center {
				background: blue;
			}
			.layout.grid .right {
				background: yellow;
			}
		</style>
		<article class="left-right-center>
			<div class="left"></div>
			<div class="center">
				<h1>grid解决方案</h1>
			</div>
			<div class="right"></div>
		</article>
	</section>
</body>
```

1. 5种方案的优缺点

	浮动：脱离文档流，需要清除浮动，优点是兼容性比较好
	
	绝对定位：好处快捷，缺点脱离文档流，可使用性比较差
	
	flex布局: 比较完美，移动端采用flex布局
	
	表格布局：兼容性好，缺点单元格高度会一起变化
	
	网格布局：比较简洁，新的技术

2. 他们之间的比较，去掉高度，考虑纵向，哪个方案不适用

	flex和table布局可以用
	
三栏布局：

* 左右宽度固定，中间自适应
* 上下高度固定，中间自适应

两栏布局：

* 左宽度固定，右自适应
* 右宽度固定，左自适应
* 上高度固定，下自适应
* 下高度固定，上自适应


### CSS盒模型
* 基本概念：标准模型 + IE模型

* 标准模型和IE模型的区别

标准模型的宽度指的是content,不包含padding和border

IE模型的宽度包含content、padding和border

* CSS是如何设置这两种模型的

box-sizing: content-box; --标准模型，浏览器默认
box-sizing: border-box;  --IE模型

JS如何设置获取盒模型对应的宽和高

dom.style.width/height -- 只能取到内联样式

dom.currentStyle.width/height -- 拿到渲染后的样式，只是IE支持

window.getCurrentStyle(dom).width/height -- 兼容FF、chrome，通用性更好

dom.getBoundingClientRect().width/height -- 计算元素的绝对位置, 拿到left、right、top、bottom

实例题（根据盒模型解释边距重叠）

```
<head>
<style>
	* {
		margin: 0;
		padding: 0;
	}
</style>
</head>
<body>
	<section id="sec">
		<style>
			#sec {
				background: #f00;
				overflow: hidden;
			}
			.child {
				height: 100px;
				margin-top: 10px;
				background: yellow;
			}
		</style>
		<article class="child"></article>
	</section>
</body>
```
设置了`overflow: hidden`,父元素的高度为110，没加为100

父子元素和兄弟元素的边距重叠，原则是取两者之间的最大值

加了`overflow: hidden`相当于给父级元素创建BFC（块级格式化上下文）


BFC（边距重叠解决方案）

原理：

1. BFC元素的垂直方向的边距会发生重叠
2. BFC的区域不会与浮动元素的box重叠，清除浮动
3. BFC在页面中是一个独立的容器，外面的元素不会影响里面的元素，里面的元素也不会影响外面的元素
4. 计算BFC高度的时候，浮动元素也会参与计算

如何创建：

1. 添加overflow为hidden或者auto
2. 设置浮动
3. postion值是除static或着reltive以外
4. display为inline-block或者table-cell、table

BFC的使用场景

```
<head>
<style>
	* {
		margin: 0;
		padding: 0;
	}
</style>
</head>
<body>
	<!--BFC垂直方向边距重叠-->
	<section id="margin">
		<style>
			#margin {
				background: pink;
				overflow: hidden;
			}
			#margin>p {
				margin: 5px auto 25px;
				background: red;
			}
		</style>
		<p>1</p>
		<div style="overflow: hidden">
			<p>2</p>
		</div>
		<p>3</p>
	</section>
</body>
```
p元素垂直方向上上下都有边距，这时候发生边距重叠，边距合并为一个值，即两者之间的最大值

消除重叠，给子元素加一个父级元素，给父级元素创建BFC，`overflow：hidden`

布局上的应用--两栏布局，左边宽度固定，右边自适应

```
<!--BFC不与float重叠-->
<section id="layout">
	<style>
		#layout {
			background: red;
		}
		#layout .left {
			float: left;
			width: 100px;
			height: 100px;
			background: pink;
		}
		#layout .right {
			height: 110px;
			background: #ccc;
			overflow: auto;
		}
	</style>
	<div class="left"></div>
	<div class="right"></div>
</section>
```
right如果不加BFC，overflow:hidden，那么右边块会和左边块重叠

```
<!--BFC子元素即使是float也会参与高度计算-->
<section id="float">
	<style>
		#float {
			background: red;
			overflow: hidden;
			float: left;
		}
		.float {
			float: left;
			font-size: 30px;
		}
	</style>
	<div class="float">我是浮动元素</div>
</section>
```
给父元素加BFC，即可清除浮动


### DOM事件

#### DOM事件的级别

DOM0 `element.onclick = function() {}`

DOM2 `element.addEventListener('click',function() {}, false)`

DOM3 `element.addEventListener('keyup',function() {}, false)` 增加鼠标，键盘事件等

#### DOM事件模型

捕获和冒泡

#### DOM事件流

事件通过捕获到达目标元素，然后再从目标元素上传到window

#### 描述DOM事件捕获的具体流程

window->document->html->body<-目标元素


```
<body>
	<div id="ev">
		<style>
			#ev {
				width: 300px;
				height: 100px;
				background: red;
				color: #fff;
				line-height: 100px;
			}
		</style>
		目标元素
	</div>
	<script>
		var ev = document.getElementById('ev');
		window.addEventListener('click', function() {
			console.log('widow captrue');
		}, true)   // 捕获阶段
		
		document.addEventListener('click', function() {
			console.log('document captrue');
		}, true) 
		
		document.documentElement.addEventListener('click', function() {
			console.log('html captrue');
		}, true) 
		
		document.body.addEventListener('click', function() {
			console.log('body captrue');
		}, true) 
		
		ev.addEventListener('click', function() {
			console.log('ev captrue');
		}, true) 
	</script>
</body>
```
捕获的流程一定是从window开始从上往下按顺序执行

冒泡的只需要改成false即可，方向相反，从下往上

#### Event对象的常见应用

1. event.preventDefault() 阻止浏览器默认行为
2. event.stopPropagation() 阻止事件冒泡
3. event.stopImmediatePropagation() 同一个按钮添加两个点击事件，如果在a中添加这个事件，就会阻止b事件触发
4. event.currentTarget  获取当前绑定的事件
5. event.target	获取当前被点击的元素

#### 自定义事件
```
<body>
	<div id="ev">
		<style>
			#ev {
				width: 300px;
				height: 100px;
				background: red;
				color: #fff;
				line-height: 100px;
			}
		</style>
		目标元素
	</div>
	<script>
		var ev = document.getElementById('ev');
		var eve = new Event('test')
		ev.addEventListener('custome', function () {
			console.log('test dispatch')
		})
		ev.dispatchEvent('eve')
	</script>
</body>
```

### HTTP协议类
* HTTP协议的主要特点
	
简单快速（url资源是固定的）

灵活（http有头部分，通过http协议完成不同数据的传输）

无连接（传输一次就会断掉）

无状态（无法区分两次连接的身份）

* HTTP报文的组成部分
	请求报文：请求行，请求头，空行，请求体
	
	请求行：http方法，页面地址，http协议，版本   GET / HTTP/1.1
	
	请求头：key，value值告诉服务器端要请求哪些类型内容  host往下都是
	
	空行：告诉服务器端往下解析请求体
	
	请求体：数据部分
	
	响应报文：状态行，响应体，空行，响应体

* HTTP方法
	GET -- 获取资源
	
	POST -- 传输资源
	
	PUT -- 更新资源
	
	DELETE -- 删除资源
	
	HEAD -- 获得报文首部

* POST和GET的区别

	* GET在浏览器回退时时无害的，而POST会再次提交请求（重点）
	* GET产生的URL可以被收藏，post不可以
	* GET请求会被浏览器主动缓存，POST不会，除非手动设置（重点）
	* GET请求参数会被完整保留在浏览器历史记录里，而POST参数不会被保留（重点）
	* GET请求在URL传送的参数室友长度限制的，太长会被截断，而POST没有限制（重点）
	* GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息
	* GET参数通过URL传递，POST放在Request body中（重点）
	
* HTTP状态码

	* 1xx: 表示请求已接收，继续处理
	* 2xx：成功-请求成功
	* 3xx: 重定向，要完成请求必须进行更进一步的操作
	* 4xx：客户端错误-请求有语法错误或请求无法实现
	* 5xx：服务器错误-服务器未能实现合法的请求

	* 200 OK：客户端请求成功
	* 301 Moved Permanently: 所请求的页面已经转移到新的url
	* 302 Found：所请求的页面已经临时转移到新的url
	* 304 Not Modified: 客户端有缓冲的文档并发出一个条件性的请求，服务器告诉客户，原来缓冲的文档还可以继续使用
	* 401：客户端有语法错误，不能被服务器所理解
	* 402： 请求未授权
	* 403： 对被请求页面的访问被禁止
	* 404：请求资源不存在
	* 500：服务器发生不可预期的错误原来缓冲的文档还可以继续使用
	* 503：请求未完成，服务器临时过载或当机，一段时间后可能恢复正常
	
* 什么是持久连接(前提是1.1版本，1.0不支持）

	http协议采用请求-应答模式，当使用普通模式，即非keep-alive模式时，每个请求/应答客户端和服务器端都要新建一个连接，完成之后立即断开连接（HTTP协议为无连接的协议）
	
	当使用keep-alive模式（又称持久连接、连接重用）时，keep-alive功能是客户端到服务器端的连接持续有效，当出现对服务器端的后续请求时，keep-alive功能避免了简历或者重新建立连接。
	
* 什么是管线化

	在使用持久连接的情况下，请求1 -> 响应1 -> 请求2 -> 响应2 -> 请求3 -> 响应3
	
	管线化，将请求打包一次传输，将响应打包一次传输，某个连接上的消息变成这样：
	
	请求1 -> 请求2 -> 请求3 -> 响应1 -> 响应2 -> 响应3
	
	* 管线化通过持久连接完成，仅http/1.1支持
	* 只有GET和HEAD请求可以进行管线化，POST有所限制
	* 初次创建时不应启动管线限制。

	
### 原型链类

* 创建对象有几种方法

	```
	// 第一种方式：字面量
    var o1 = { name: 'o1' }
    var o2 = new Object({ name: 'o2' })
    // 第二种方式：通过构造函数
    var M = function(name) { this.name = name }
    var o3 = new M('o3')
    // 第三种：Object.create
    var P = { name: 'p' }
    var o4 = Object.create(p)
	```
* 原型、构造函数、实例、原型链

	构造函数通过new生成实例，任何函数都可以作为构造函数，声明函数都有一个prototype属性，这个属性会初始化一个对象，即原型对象，原型对象有一个构造器，constructor，默认指向声明的函数
	
	实例对象的__proto__属性指向原型对象
	
	原型链：实例对象往上找构造实例相关联的对象，一直到object.prototype，这是整个原型链的顶端。
	
	通过原型链找到原型对象的方法，原型对象的方法被不同实例所共有的
	
	寻找实例的方法，现在实例本身上寻找，如果没有找到就通过__proto__到原型对象上找,如果还没找到，再通过__proto__往上一级的原型对象上找，一直到object.prototype,如果还没找到，就告诉这个实例没有找到方法或者属性。 
	
	函数才会有prototype，对象是没有prototype
	
	只有实例对象有__proto__，函数也是对象，也有__proto__
	
	
	
	```
	//接上面例子
	M.prototype.constructor === M   // true
	o3.__proto__ === M.prototype
	
	M.prototype.say = function() {
    	console.log('say hi')
    }

    var o5 = new M('o5')
    
    o3.say() // say hi
    o5.say() // say hi
    
    M.__proto__ === Function.prototype // M的构造函数是function，M普通函数是Function构造函数的实例
	```
* instanceof的原理


## 二面/三面

### 渲染机制
* 什么是DOCTYPE及作用
	DTD是一系列的语法规则，用来定义XML或XHTML的文件类型。浏览器会使用它来判断文档类型，决定使用何种协议来解析，以及切换浏览器模式。
	
	DOCTYPE是用来声明文档类型和DTD规范的，一个主要的用途便是文件的合法性验证。如果文档代码不合法，那么浏览器解析就会出一些差错。
	
	HTML5  <!DOCTYPE html>
* 浏览器是怎么渲染的

	浏览器拿到HTML和CSS，HTML转成DOM Tree，css转成CSSDOM Tree，最终合并成Render tree，同事进行布局位置计算，然后进行描绘，最后显示。
	
* 重排Reflow
	DOM结构中各个元素都有自己的盒子模型，这些都需要浏览器根据样式来进行计算并根据计算结果将元素放到它该出现的位置，这个过程就叫reflow。
	
	触发Reflow
	
	修改DOM，移动DOM，修改样式，resize窗口或者滚动，修改网页的默认字体
	
* 重绘Repaint

	当盒子的位置，大小以及其他属性，例如颜色，字体大小等都确定下来后，浏览器于是便把这些元素按照各自的特性绘制了一遍，也是页面内容就出现了，这个过程就叫repaint。
	
	触发repaint
	
	DOM改动，CSS改动
	
	如何避免最小程度的repaint：添加节点，一次性添加。
	
* 布局Layout

### JS运行机制
```
console.log(1)
setTimeout(function() {
	console.log(3)
}, 0)
console.log(2)
// 1  2  3
```
JS是单线程的，一个时间内JS只能干一件事，任务队列分同步任务和异步任务，setTimeout是异步任务，异步任务要挂起，先不执行，先执行同步任务，同步任务执行完毕再触发异步任务。

```
console.log('A')
while(true) {

}
console.log('B')
// 输出'A'
```
while是同步任务，会不断的循环，所以不会执行到B

```
for (var i=0;i<4;i++) {
	setTimeout(function() {
		console.log(i)
	}, 1000)
}
// 输出4个4
```
setTimeout是异步任务，每次循环setTimeout就会被挂起，放入任务队列中，都是一秒后执行，所以有四个setTimeout任务，这里for循环是同步任务，所以会先执行for循环，for循环执行结束之后，i变量被重复声明，其结果是最后一次for循环的结果，也就是4，然后再执行4次setTimeout任务，输出4个4。

Event loop(事件循环)

浏览器的JS引擎遇到setTimeout，识别是一个异步任务，它不会把它放在异步栈里面，先拿走，同步任务结束之后，同步任务栈空了之后，setTimeout定时时间到了，会把setTimeout任务放到异步任务栈里，然后在运行栈中执行，结束之后再去异步任务栈中监听有没有异步任务，如此循环。

异步任务

* setTimeout和setInterval
* DOM事件
* ES6中的Promise

### 页面性能

### 错误监控


## 三面/四面

* 业务能力
	做过什么业务，负责的业务有什么业绩，使用了什么技术方案，突破了什么技术难点，遇到了什么问题，最大的收获是什么

* 团队协作能力 
* 事务推动能力（跨部门） 
* 带人能力 
* 其他能力

## 终面
* 乐观积极
* 主动沟通
* 逻辑顺畅
* 上进有责任心
* 有主张，做事果断
* 职业竞争力
	1. 业务能力，可以做到行业第一
	2. 思考能力，对同一件事情从不同的角度考虑，找到最优解
	3. 学习能力，不断的学习新的技术和业务，沉淀，总结
	4. 无上限的付出，对于无法解决的问题可以熬夜、加班

* 职业规划
	1. 目标是在业务上称为专家，在技术上称为行业的大牛
	2. 近阶段的目标，不断学习积累各方面的经验，以学习为主
	3. 做几件有价值的事情，比如开源作品，技术框架等。
	4. 方式方法，先完成业务上的主要问题，做到极致，然后逐步向目标靠拢
	5. 希望公司有技术分享，给新人以技术分享
	6. 多赞美公司，多赞美HR