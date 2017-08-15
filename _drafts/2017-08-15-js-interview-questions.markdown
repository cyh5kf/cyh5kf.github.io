---
layout: post
title: Js经典面试题
date: 2017-08-15 12:00:00.000000000 +08:00
---

对于一个面试题，首先要思考题目的考点

## 知识点讲解，变量类型和变量计算：

JS中定义了6中基本数据类型：null, undefined, number, string, boolean, object
	
#### 值类型和引用类型
	
```
// 值类型
var a = 100;
var b = a;
a = 200;
console.log(b) //100
```
```
// 引用类型
var a = {age: 20};
var b = a;
b.age = 21;
console.log(a.age) //21
```
值类型： undefined、number、string、boolean，在内存中，每个值都存在变量内存中的一个位置。
	
引用类型：对象、数组、函数，变量只是一个指针，指向一个内存中的对象，可以无限制扩展属性，如果属性比较多，会出现内存占用比较大的问题，如果复制一份给另一个变量会占用更多的空间，所以引用类型为了让内存中共用空间，节省内存空间，使用引用的方式。
	
typeof能区分6种类型：undefined, string, number, boolean, object, function
	
null也是object类型，但是定义为空，占了一个位置，没有指向任何引用类型
	
typeof只能区分值类型的详细类型，无法区分引用类型
	
#### 变量计算-强制类型转换
这里主要讲的是值类型的变量计算
	
1. 字符串拼接
	
	```
	var a = 100 + 10 // 110
	var b = 100 + '10' //'10010'
	```
2. == 运算符

	```
	100 == '100' // true
	0 == '' // true
	null == undefined // true
	```
3. if语句
	
	```
	var a = true
	if (a) {
		// ...
	}
	var b = 100
	if (b) {
		// ...
	}
	var c =''
	if (c) {
		// ...
	}
	```
	if语句下，undefined, false, null, '', 0, NaN都会转化成false
4. 逻辑运算
	
	```
	console.log(10 && 0) // 0
	console.log('' || 'abc') //'abc'
	console.log(!window.abc) //true
	//把一个值转换为布尔值
	var a = 100;
	console.log(!!a)
	```
	
## 对应题目：
* #### JS使用typeof能得到什么类型（JS变量类型）
	typeof能区分6种类型：undefined, string, number, boolean, object, function

* #### 何时使用 === 何时使用 ==（强制类型转换）
	```
	// 首先a需要定义
	if (obj.a == null) {
		// 这里相当于 obj.a === null || obj.a === undefined, 简写形式
		// 这是jquery推荐写法
	}
	```
	除了这种情况，其他都用 ===
	
* #### JS有哪些内置函数——数据封装类对象
	Object, Array, Boolean, Number, String, Function, Date, RegExp, Error
	
* #### JS变量按照存储方式区分为哪些类型，并描述其特点
	值类型和引用类型
	
	值类型每一个变量都占据一个内存空间。引用类型多个变量共用一个内存块多个变量通过指针指向一个内存块，为了节省内存空间，值赋值只是变量指针赋值，而不是真正的拷贝，所以值的修改会相互干预
	
* #### 如何理解JSON
	JSON是一个js对象，不是函数，同时也是数据格式，有两个API
	
	```
	JSON.stringify({a: 10, b: 20})  // 把对象变成字符串
	JSON.parse('{"a": 10, "b": 20}') // 把字符串变成对象
	```

* #### window.onload和DOMContentLoaded的区别（浏览器的渲染过程）

* #### 用JS创建10个`<a>`标签，点击的时候弹出来对应的序号（作用域）

* #### 简述如何实现一个模块加载器，实现类似一个require.js的功能（JS模块化）

* #### 实现数组的随机排序（JS基础算法）

## 知识点讲解，原型和原型链：

#### 构造函数

```
function Foo(name, age) {
	this.name = name;
	this.age = age;
	this.class = 'class-1'
	// return this  // 默认有这一行
}
var f = new Foo('zhangsan', 20)
// var f1 = new Foo('lisi', 22) // 创建多个对象
```
new一个对象的过程，首先传参数到构造函数，然后this变成空对象，this属性赋值，赋值之后默认`return this`，这样f就具有了`f.name = 'zhangsan'`的属性

#### 构造函数-扩展
* var a = {} 其实是var a = new Object()的语法糖
* var a = [] 其实是var a = new Array()的语法糖
* function Foo(){} 其实是var Foo = new Function()的语法糖
* 使用instanceof判断一个函数是否是一个变量的构造函数  判断是否为数组(instanceof Array)

#### 原型规则和示例
5条原型规则

1.所有的引用类型（数组，对象，函数），都具有对象属性，即可自由扩展属性（除了“null”以外）

```
var obj = {}; obj.a = 100;
var arr = []; arr.a = 100;
function fn() {}
fn.a = 100;
```
2.所有的引用类型（数组，对象，函数），都有一个<mark>__proto__</mark>属性，属性值是一个普通的对象,`_proto_`叫做隐式原型

```
console.log(obj.__proto__);
console.log(arr.__proto__);
console.log(fn.__proto__);
```
3.所有的函数，都有一个<mark>prototype</mark>属性，属性值也是一个普通的对象，`prototype`叫做显示原型

```
console.log(fn.prototype)
```
4.所有的引用类型（数组，对象，函数），`__proto__`属性值指向它的构造函数的“ prototype ”属性值

```
console.log(obj.__proto__ === Object.prototype)
```
5.当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的`__proto__`(即它的构造函数的prototype)中寻找。

```
// 构造函数
function Foo(name, age) {
	this.name = name
}
Foo.prototype.alertName = function() {
	alert(this.name)
}
// 创建示例
var f = new Foo('zhangsan')
f.printName = function() {
	console.log(this.name);
}
// 测试
f.printName()
f.alertName() //对应第5条规则，f本身没有alertName属性，就去寻找隐式原型即构造函数的显示原型中寻找

// 循环对象自身的属性
var item;
for (item in f) {
	// 高级浏览器已经屏蔽了来自原型的属性
	// 但是这里还是建议大家加上这个判断，是否是自身的属性，保证程序的健壮性
	if (f.hasOwnProperty(item)) {
		console.log(item)
	}
}
```

#### 原型链
```
// 接上面示例
f.toString() 
// 要去 f.__proto__.__proto__中查找
```
先查找f自身有没有这个属性，没有这个属性，再从f的隐式原型即构造函数Foo的显示原型中找，没有找到，Foo的显示原型是一个对象，在这个对象中找toString属性，没有找到，再从Object的隐式原型即Object构造函数的显示原型中找，Object显示原型是个对象，这个对象的隐式原型是个null，到这里就结束了

![原型链示意图](http://ouq0pnc4r.bkt.clouddn.com/prototype.png)

#### instanceof
用于判断引用类型属于哪个构造函数的方法

f instanceof Foo的判断逻辑是：

f的__proto__一层一层往上，能否对应到Foo.prototype

再试着判断 f instanceof Object,同理也是通过__proto__网上找

## 对应题目：
* #### 如何准确判断一个变量是数组类型

	```
	var arr = [];
	arr instanceof Array  // true
	typeof arr // object, typeof是无法判断是否为数组
	```
* #### 写一个原型链继承的例子

	```
	// 动物
	function Animal() {
		this.eat = function() {
			console.log('animal cat');
		}
	}
	// 狗
	function Dog() {
		this.bark = function() {
			console.log('dog bark');
		}
	}
	Dog.prototype = new Animal()
	// 哈士奇
	var hashiqi = new Dog()
	```
	```
	// 更加贴近实战的实例，写一个分装DOM查询的例子
	function Elem(id) {
		this.elem = document.getElementById(id);
	}
	
	Elem.prototype.html = function(val) {
		var elem = this.elem;
		if (val) {
			elem.innerHTML = val;
			return this // 链式操作
		} else {
			return elem.innerHTML;
		}
	}
	
	Elem.prototype.on = function(type, fn) {
		var elem = this.elem;
		elem.addEventListener(type, fn);
		return this;
	}
	
	var div1 = new Elem('div1');
	// console.log(div1.html());
	div1.html('<p>hello world!</p>');
	div1.on('click', function() {
		alert('clicked');
	})
	// 也可以链式操作
	// div1.html('<p>hello world!</p>').on('click', function() {
		alert('clicked');
	}).html('<p>javascript</p>');
	```
* #### 描述new一个对象的过程

	```
	function Foo(name, age) {
		this.name = name;
		this.age = age;
		this.class = 'class-1'
		// return this  // 默认有这一行
	}
	var f = new Foo('zhangsan', 20)
	// var f1 = new Foo('lisi', 22) // 创建多个对象
	```
	1. 创建一个新对象
	2. this指向这个新对象
	3. 执行代码，即对this赋值
	4. 返回this
* #### zepto（或其他框架）源码中如何使用原型链

	读源码先从网上搜一些资料，比如jquery源码分析，慕课网有zepto设计和源码分析
	
	阅读源码在面试中会有加分
	
	






