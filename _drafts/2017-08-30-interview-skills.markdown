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

#### Event对象的常见应用

1. event.preventDefault() 阻止浏览器默认行为
2. event.stopPropagation() 阻止事件冒泡
3. event.stopImmediatePropagation() 同一个按钮添加两个点击事件，如果在a中添加这个事件，就会阻止b事件触发
4. event.currentTarget  获取当前绑定的事件
5. event.target	获取当前被点击的元素

#### 自定义事件
```
var eve = new Event('custome')
ev.addEventListener('custome', function () {
	console.log('custome')
})
ev.dispatchEvent('eve')
```