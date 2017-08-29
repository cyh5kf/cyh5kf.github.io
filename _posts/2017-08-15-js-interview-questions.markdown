---
layout: post
title: JS面试基础知识
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

再试着判断 f instanceof Object,同理也是通过__proto__往上找

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
	
	
## 知识点讲解，作用域和闭包：
#### 执行上下文

* 范围：一段`<script>`或者一个函数
* 全局：变量定义、函数声明 一段`<script>`
* 函数：变量定义、函数声明、this、arguments  函数

PS：注意“函数声明”和“函数表达式”的区别

```
// 全局作用域
consolo.log(a); // undefined
var a = 100;  //变量声明会提前，但是不会赋值

fn('zhangsan') // 'zhangsan' 20
function fn(name) { //函数声明会提前
	// 函数作用域
	console.log(this);  // window对象
	console.log(arguments);  // [zhangsan ...]
	
	age = 20;
	console.log(name, age);
	var age;
	
	bar(100);  // 100
	function bar(num) {
		console.log(num)
	}
}
```

```
fn()  //先把函数声明提前
function fn() {
	// 函数声明
}

fn1() // 这里fn1是undefined,执行会报错
var fn1 = function() {
	// 函数表达式
}
```

#### this
this要在执行时才能确认，定义时无法确认

```
var a = {
	name: 'A',
	fn: function() {
		console.log(this.name);
	}
}
a.fn(); // 'A'  this === a
a.fn.call({name: 'B'})  // 'B'   this === {name: 'B'}
var fn1 = a.fn;
fn1()  //''  this === window
```
* 作为构造函数执行

```
function Foo(name) {
	this.name = name
}
var f = new Foo('zhangsan')
```
* 作为对象属性执行

```
var obj = {
	name: 'A',
	printName: function() {
		cosole.log(this.name);
	}
}
obj.printName()  // 'A'
```
* 作为普通函数执行

```
function fn() {
	console.log(this)  // window
}
fn()
```
* call apply bind

```
function fn1(name, age) {
	alert(name)
	console.log(this)  // window
}
fn1.call({x: 100}, 'zhangsan', 20)  // 第一个参数作为this， 之后的参数作为函数的参数传入
fn1.apply({x: 100}, ['zhangsan', 20])  //apply会把后面的参数当做数组传递

var fn2 = function(name, age) {
	alert(name)
	console.log(this)  // window
}.bind({y: 200})  //bind指定this指向的对象
fn2('zhangsan', 20)
```

#### 作用域
* JS没有块级作用域
* 只有函数和全局作用域

```
// 无块级作用域
if (true) {
	var name = 'zhangsan'
}
console.log(name)

// 函数和全局作用域
var a = 100
function fn() {
	var a = 200
	console.log('fn', a)
}
console.log('global', a)
fn()
```

#### 作用域链

```
var a = 100;
function fn() {
	var b = 200;
	
	// 当前作用域没有定义的变量，即‘自由变量’
	// 当前作用域没有定义就去父级作用域（全局作用域）去找
	// 函数的父级作用域是函数定义时所在的父级作用域，不是函数执行时所在的父级作用域
	console.log(a) // 100
	console.log(b) // 200
}
fn()
```
```
//当前作用域没有定义，一直往父级作用域找，直到全局作用域
var a = 100;
function F1() {
	var b = 200;
	function F2() {
		var c = 300;
		console.log(a); // a是自由变量
		console.log(b); // b是自由变量
		console.log(c);
	}
	F2()
}
F1()
```

#### 闭包
闭包的使用场景

* 函数作为返回值(上面这个demo)

```
function F1() {
	var a = 100;
	
	// 返回一个函数（函数作为返回值）
	return function() {
		console.log(a)
	}
}
// f1得到一个函数
var f1 = F1();
var a = 200;
f1();
```
* 函数作为参数传递
函数的作用域始终是函数定义时候的作用域

```
function F1() {
	var a = 100;
	
	// 返回一个函数（函数作为返回值）
	return function() {
		console.log(a)
	}
}
// f1得到一个函数
var f1 = F1();
function F2(fn) {
	var a = 200;
	fn()
}
F2(f1)
```

## 对应题目：
* #### 说一下对变量提升的理解
	变量定义

	函数声明（注意和函数表达式的区别）

* #### 说明this几种不同的使用场景
	* 作为构造函数执行
	* 作为对象属性执行
	* 作为普通函数执行
	* call apply bind

* #### 创建10个`<a>`标签，点击的时候弹出对应的序号
	自执行函数，就是不用调用，只要定义完成，就立即执行的函数

	```
	var i;
	for (i = 0; i < 10; i++) {
		(function(i) {
			var a = document.createElement('a');
			a.innerHTML = i + '<br>';
			a.addEventListener('click', function(e) {
				e.preventDefault();
				alert(i);  // i是自由变量，要去父作用域获取值
			})
			document.body.appendChild(a);
		})(i)
	}
	```

* #### 如何理解作用域
	* 自由变量
	* 作用域链，即自由变量的查找
	* 闭包的两个场景

* #### 实际开发闭包的应用
	```
	// 闭包实际应用中主要用于封装变量，收敛权限
	// 这里把存储的数据封装在函数内部，函数外部是拿不到修改变量的权限，外部拿到的只是返回的函数
	function isFirstLoad() {
		var _list = [];  // 带有下划线的变量表明是自定义私有变量
		return function(id) {  // _list是自由变量，要去父级作用域找
			if (_list.indexOf(id) >= 0) {
				return false;
			} else {
				_list.push(id);
				return true;
			}
		}
	}
	
	// 使用
	var firstLoad = isFirstLoad();
	firstLoad(10) // true
	firstLoad(10)  // false
	firstLoad(20) // true
	```

## 知识点讲解，异步和单线程：
#### 什么是异步(对比同步)

```
console.log(100);
// 异步操作
setTimeout(function() {
	console.log(200)
}, 1000)
console.log(300)
```
```
// 同步操作
// 同步会阻塞下面代码执行
console.log(100);
alert(200);
console.log(300);
```
#### 何时使用异步
* 在可能发生等待的情况
* 等待过程中不能像alert一样阻塞程序的执行
* 因此，所有的“等待的情况”都需要异步

#### 前端使用异步的场景
* 定时任务：setTimeout, setInterval
* 网络请求，动态`<img>`加载
* 事件绑定

```
console.log('start')
$.get('./data.json', function(data1) {
	console.log(data1);
})
console.log('end')
```
```
console.log('start');
var img = document.createElement('img')
img.onload = function() {
	console.log('loaded')
}
img.src = '/xxx.png'
console.log('end')
```
```
console.log('start')
document.getElementById('btn1').addEventListener('click', function() {
	alert('clicked')
})
console.log('end')
```

#### 异步和单线程

```
console.log(100)
setTimeout(function() { //所有异步的操作都会暂存起来，不会立即执行
	console.log(200)
})
console.log(300)  // 待所有程序执行完，处于空闲状态，会立马看有没有暂存起来要执行
//发现暂存起来的setTimeout函数无需等待时间，就立即过来执行
```
单线程是程序只能一个一个依次执行，不能同时干两件事

## 对应题目：
* #### 同步和异步的区别是什么？分别举一个同步和异步的例子
	* 同步会阻塞代码执行，而异步不会
	* alert是同步， setTimeout是异步

* #### 一个关于setTimeout的面试题
	```
	console.log(1)
	setTimeout(function() {
		console.log(2)
	}, 0)
	console.log(3)
	setTimeout(function() {
		console.log(4)
	}, 1000)
	console.log(5)
	```

* #### 前端使用异步的场景有哪些？
	* 定时任务：setTimeout, setInterval
	* 网络请求，动态`<img>`加载
	* 事件绑定

## 其他JS知识：
#### 日期
```
Date.now() // 获取当前时间的毫秒数
var dt = new Date()
dt.getTime() // 获取毫秒数
dt.getFullYear() // 年
dt.getMonth() // 月(0 - 11)
dt.getDate() // 日(0 - 31)
dt.getHours() // 小时(0 - 23)
dt.getMinutes() // 分钟(0 - 59)
dt.getSeconds() // 秒(0 - 59)
```

#### Math
获取随机数 Math.random()


#### 数组API
* `forEach` 遍历所有元素

```
var arr = [1, 2, 3]
arr.forEach(function(item, index) {
	// 遍历数组的所有元素
	console.log(index, item)
})
```

* `every` 判断所有元素是否都符合条件

```
var arr = [1, 2, 3]
var result = arr.every(function(item, index) {
	// 用来判断所有的数组元素，都满足一个条件
	if (item < 4) {
		return true
	}
})
console.log(result)
```

* `some` 判断是否有至少一个元素符合条件

```
var arr = [1, 2, 3]
var result = arr.some(function(item, index) {
	// 用来判断所有的数组元素，都满足一个条件
	if (item < 2) {
		return true
	}
})
console.log(result)
```

* `sort` 排序

```
var arr = [1, 4, 2, 3, 5]
var arr2 = arr.sort(function(a, b) {
	// 从小到大排序
	return a - b
	
	//从大到小排序
	// return b - a	
})
console.log(arr2)
```

* `map` 对元素重新组装，生成新数组

```
var arr = [1, 2, 3, 4]
var arr2 = arr.map(function(item, index) {
	// 将元素重新组装，并返回
	return '<b>' + item + '<b>'
})
console.log(arr2)
```

* `filter` 过滤符合条件的元素

```
var arr = [1, 2, 3]
var arr2 = arr.filter(function(item, index) {
	// 通过某一个条件过滤数组
	if (item >= 2) {
		return true
	}
})
console.log(arr2)
```

#### 对象API

```
var obj = {
	x: 100,
	y: 200,
	z: 300
}
var key
for( key in obj) {
	// 注意这里的hasOwnProperty,在讲原型的时候讲过
	if (obj.hasOwnProperty(key)) {
		console.log(key, obj[key])
	}
}
```

## 对应题目：

* #### 获取2017-xx-xx 格式的日期

	```
	function formatDate(dt) {
		if (!dt) {
			dt = new Date()
		}
		var year = dt.getFullYear()
		var month = dt.getMonth() + 1
		var date = dt.getDate()
		if (month < 10) {
			// 强制类型转换
			month = '0' + month
		}
		if (date < 10) {
			//强制类型转换
			date = '0' + date
		}
		//强制类型转换
		return year + '-' + month + '-' + date
	}
	var dt = new Date()
	var formatDate = formatDate(dt)
	console.log(formatDate)
	```

* #### 获取随机数，要求长度一致的字符串格式

	```
	var random = Math.random()
	random = random + '0000000000' //后面加上10个零
	random = random.slice(0, 10)
	// random = random.toFixed(8)
	console.log(random)
	```

* #### 写一个能遍历对象和数组的通用forEach函数

	```
	function forEach(obj, fn) {
		var key
		if (obj instanceof Array) {
			obj.forEach(function(item, index) {
				fn(index, item)
			})
		} else {
			// 不是数组就是对象
			for (key in obj) {
				if (obj.hasOwnProperty(key)) {
					fn(key, obj[key])
				}
			}
		}
	}
	
	var arr = [1, 2, 3]
	// 注意这里的参数顺序换了，为了和对象的遍历格式保持一致
	forEach(arr, function(index, item) {
		console.log(index, item)
	})
	
	var obj = {x: 100, y: 200}
	forEach(obj, function(key, value) {
		console.log(key, value)
	})
	```
	
## JS-web-api
只管定义用于浏览器JS操作页面API和全局变量（window、document、navigator.userAgent...）
常说的JS包含两个部分：JS基础知识（ECMA标准）和JS-Web-API(W3C标准)

## DOM操作(document object model)
#### DOM本质
DOM树，一种浏览器可识别的数据结构，由各种标签组成，方便js的API去操作。

#### DOM节点操作
面试中不要说用起来很熟，但不知道实现原理的东西

* 获取DOM

	```
	document.getElementById()
	
	document.getElementsByTagName()
	
	document.querySelectorAll()
	
	document.querySelector()
	```
* property

	JS对象的属性
	
	```
	var obj = {x:100, y:200};
	console.log(obj.x); // 100
	
	var p = document.getElementsByTagName('p')[0];
	cosole.log(p.nodeName) // P
	```
* attribute
	HTML标签的属性

	```
	var pList = document.querySelector('p');
	var p = pList[0];
	p.getAttribute('data-name');
	p.setAttribute('data-name', 'imooc');
	p.getAttribute('style');
	p.setAttribute('style', 'font-size:30px');
	```

#### DOM结构操作
* 新增节点

	```
	var div1 = document.getElementById('div1');
	// 添加新节点
	var p1 = document.createElement('p');
	p1.innerHTML = 'this is p1';
	div1.appendChild(p1); // 添加新创建的元素
	// 移动已有节点
	var p2 = document.getElementById('p2');
	div1.appendChild(p1);
	```
* 获取父元素
* 获取子元素
* 删除节点

	```
	var div1 = document.getElementById('div1');
	var parent = div1.parentElement;
	
	var child = div1.childNodes;
	div1.removeChild(child[0]);
	
	div.childNode[0].nodeType  //3	3表示空格或者换行符
	div.childNode[1].nodeType  //1  1表示html标签节点
	div.childNode[0].nodeName  //#text
	div.childNode[1].nodeName  // P
	```

## 对应题目：
* #### DOM是哪种基本的数据结构？
树

* #### DOM操作的常用API有哪些？
获取DOM节点，以及节点的property和Attribute

获取父节点，获取子节点

新增节点，删除节点

* #### DOM节点的attr和property 有何区别？

property只是一个JS对象的属性的修改
Attribute是对html标签属性的修改

## BOM操作（Browser Object Model）
#### navigator

```
var ua = navigator.userAgent
var isChrome = ua.indexOf('Chrome')
console.log(isChrome)
```
#### screen

```
console.log(screen.width)
console.log(screen.height)
```
#### location

```
console.log(location.href) // 整个url
console.log(location.protocol) // 协议 'http:' 'https:'
console.log(location.host) //域名
console.log(location.pathname) // 路径  '/learn/199'
console.log(location.search) // ?后面的参数
console.log(location.hash) // #后面的hash
```
#### history

```
history.back()
history.forward()
```
## 对应题目：
* #### 如何检测浏览器的类型

* #### 拆解url的各部分

## 事件
#### 通用事件绑定

```
var btn = document.getElementById('btn1')
btn.addEventListener('click', function (event) {
	console.log('click')
})

function bindEvent(elem, type, fn) {
	elem.addEventListener(type, fn)
}
var a = document.getElementById('link1')
bindEvent(a, 'click', function(e) {
	e.preventDefault() // 阻止默认行为
	alert('clicked')
})
```
关于IE低版本的兼容性

IE低版本使用attachEvent绑定事件，和W3C标准不一样

IE低版本使用量已经非常少了，很多网站都不支持了

对IE低版本的兼容性：了解即可，无需深究

如果对IE低版本要求苛刻的面试，果断放弃
#### 事件冒泡

```
<div id="div1">
	<p id="p1">激活</p>
	<p id="p2">取消</p>
</div>
<div id="div1">
	<p id="p3">取消</p>
	<p id="p4">取消</p>
</div>

var p1 = document.getElementById('p1')
var body = document.body
bindEvent(p1, 'click', function() {
	e.stopPropagation() // 阻止事件冒泡
	alert('激活')
})
bindEvent(body, 'click', function(e){
	alert('取消')
})
```
#### 代理
好处：

代码简洁

减少浏览器内存占用

```
<div id="div1">
	<a href="#">a1</a>
	<a href="#">a2</a>
	<a href="#">a3</a>
	<a href="#">a4</a>
	<!--会随时新增更多的a标签-->
</div>

var div1 = document.getElementById('div1')
div1.addEventListener('click', function(e) {
	var target = e.target //可以拿到真实触发事件的元素
	if (target.nodeName === 'A') {
		alert(target.innerHTML)
	}
})
```
## 对应题目：
* #### 编写一个通用的事件监听函数
	```
	// selector表示是否使用代理
	function bindEvent(elem, type, selector, fn) {
		if (fn === null) {
			fn = selector
			selector = null
		}
		elem.addEventListener(type, function(e) {
			var target
			if (selector) { // 使用代理
				target = e.target
				if (target.matches(selector)) { //target是否符合代理目标的元素
					fn.call(target, e)
				}
			} else { // 不使用代理
				fn(e)
			}
		})
	}
	
	// 使用代理
	var div1 = document.getElementById('div1')
	bindEvent(div1, 'click', 'a', function (e) {
		console.log(this.innerHTML)
	})
	
	// 不使用代理
	var a = document.getElementById('a1')
	bindEvent(div1, 'click', function (e) {
		console.log(a.innerHTML)
	})
	```

* #### 描述事件冒泡

	DOM树形结构
	
	事件冒泡
	
	阻止冒泡
	
	冒泡的应用

* #### 对于一个无线下拉加载图片的页面，如何给每个图片绑定事件

	使用代理

	代理的好处


## Ajax
#### XMLHttpRequest
```
var xhr = new XMLHttpRequest();
xhr.open("GET", "/api", false);  // false使用异步
xhr.onreadystatechange = function() {
	// 这里的函数异步执行，可参考之前的JS基础中的异步模块
	if (xhr.readyState == 4) {
		if (xhr.status == 200) {
			alert(xhr.responseText)
		}
	}
}
xhr.send(null)
```

IE兼容性问题

* IE低版本使用ActiveXObject，和W3C标准不一样
* IE低版本使用量已经非常少，很多网站早已不支持
* 建议对IE的低版本兼容性，了解即可，无需深究

#### 状态码说明
readyState

* 0 - (未初始化)还没调用send方法
* 1 - (载入)已经调用send方法，正在发送请求
* 2 - (载入完成)send方法已经执行完成，已经接收到全部响应内容
* 3 - (交互)正在解析响应内容
* 4 - (完成)响应内容解析完成，可以在客户端调用了

status

* 2xx - 表示成功处理请求。如200
* 3xx - 需要重定向，浏览器直接跳转
* 4xx - 客户端请求错误。如404
* 5xx - 服务器端错误

#### 跨域
浏览器有同源策略，不允许ajax访问其他域接口

跨域条件：协议、域名、端口，有一个不同就算跨域

可以跨域的三个标签：

* `<img src="xxx">`

	`<img>	`用于打点统计，统计网站可能是其他域

* `<link href=xxx>`

	`<link>``<script>`可以使用CDN,CDN的也是其他域

* `<script src=xxx>`

	`<script>`可以用于JSONP
	
跨域注意事项

* 所有的跨域请求都必须经过信息提供方允许
* 如果未经允许即可获取，那是浏览器同源策略出现漏洞

JSONP实现原理
利用script标签跨域加载其他域的资源

服务器端跨域设置 http header

```
// 第二个参数填写允许跨域的域名称，不建议直接写“*”
response.setHeader("Access-Control-Allow-Origin", "http://a.com", "http://b.com")
response.setHeader("Access-Control-Allow-Headers", "X-Requested-With")
response.setHeader("Access-Control-Allow-Origin", "PUT,GET,POST,DELETE,OPTIONS")

// 接受跨域的cookie
response.setHeader("Access-Control-Allow-Credentials", "true")
```

## 对应题目：

* #### 手动编写一个ajax库，不依赖第三方库

* #### 跨域的几种实现方式


## 存储
#### cookie
* 本身用于客户端和服务器端通信
* 但是它有本地存储的功能，于是就被借用
* 使用document.cookie=... 获取和修改即可

#### cookie存储的缺点
* 存储量太小，只有4KB(与服务器端通信)
* 所有http请求都带着，会影响获取资源的效率
* API简单，需要封装才能使用 document.cookie = ...

#### localStorage和sessionStorage
* HTML5专门为存储设计，最大容量为5M(不用在请求中带着)
* API简单易用
* localStorage.setItem(key, value); localStorage.getItem(key)
* IOS safari隐藏模式下，localStorage.getItem会报错，建议统一使用try-catch封装

## 对应题目：
* #### 请描述cookie,sessionStorage,和localStorage的区别
	1. 容量
	2. 是否会携带到ajax中
	3. API易用性


## 开发环境

### IDE
webstorm sublime vscode atom

### git
coding.net(国内)  github.com(国外)

### JS模块化
通过script标签引入js文件的方式不足：

1. 这些代码中的函数必须是全局变量，才能暴露给使用方，全局变量污染。
2. 依赖关系不明确

#### AMD 异步模块定义规范
工具：require.js，定义全局的define函数和require函数，依赖JS会自动异步加载

```
// a.js define定义
define(['./a-util.js'], function (aUtil) {
	return {
		printDate: function (date) {
			console.log(aUtil.aGetFormatDate(date))
		}
	}
})

// main.js  require引入
require(['./a.js'], function (a) {
	var date = new Date()
	a.printDate(date)
})

<script src="/require.min.js" data-main="./main.js"></script>
```

#### CommonJS
nodejs模块化规范，不会异步加载JS，同步一次性加载，需要构建工具支持

```
// util.js
module.exports = {
	getFormatDate: function (date, type) {
		if (type === 1) {
			return '2017-06-15'
		}
		if (type === 2) {
			return '2017年5月15日
		}
	}
}

// a-util.js
var util = require('util.js')
module.exports = {
	aGetFormatDate: function(date) {
		return util.getFormatDate(date, 2)
	}
}
```

AMD和CommonJS使用场景

* 需要使用异步加载JS，使用AMD
* 使用npm之后建议使用CommonJS



## 打包工具
webpack

## 上线回滚的流程
#### 上线
* 将测试完成的代码提交到git版本库的master分支
* 将当前服务器的代码全部打包并记录版本号，备份
* 将master分支的代码提交覆盖到线上服务器，生成版本号

#### 回滚
* 将当前服务器的代码打包并记录版本号，备份
* 将备份的上一个版本号解压，覆盖到线上服务器，并生成新的版本号

linux命令

`cat a.js`  查看文件全部内容

`head -n 1 a.js` 查看文件前一行

`tail -n 2 a.js`  查看文件后面两行

`grep '2' a.js`  在文件中搜索2

`vi a.js` 编辑文件


## 安全性
#### XSS 跨站请求攻击
在新浪博客中写一篇文章，同时偷偷插入`<script>`，攻击代码中，获取cookie，发送到自己的服务器，发布博客，有人查看博客内容，会把查看者的cookie发送到攻击者的服务器

前端替换关键字，例如替换< 为 `&lt;` > 为 `&gt;`

后端替换，最好后端替换，执行效率高

#### XSRF 跨站请求伪造
比如针对一个支付接口，要增加安全性验证，如输入指纹、密码、短信验证码

## 页面加载过程
#### 加载资源的形式
1. 	输入url（或者跳转页面）加载html
2. 输入链接
3. 加载html中的静态资源，js，css，视频，图片。。。

#### 加载一个资源的过程
1. 浏览器根据DNS服务器得到域名的IP地址
2. 向这个IP的服务器发送http请求
3. 服务器收到、处理并返回http请求
4. 浏览器得到返回内容

#### 浏览器渲染页面的过程
1. 根据html结构生成DOM Tree
2. 根据CSS生成CSSOM
3. 将DOM和CSSOM整合成RenderTree
4. 根据RenderTree开始渲染和展示
5. 遇到`<script>`时，会执行并阻塞渲染

* 为什么将css放在`head`

浏览器顺序执行，先加载css，得到CSSOM，然后再生成DOM结构的时候直接整合成RenderTree，只需要渲染一次就可以了

* 将js放在body最后

不会阻塞页面加载，能让页面更快的渲染出来，js放在最后，js能操作所有的DOM

* 图片是异步加载的

## 对应题目
* #### 从输入url到html的详细过程

* #### window.onload 和 DOMContentLoaded的区别
	
	```
	window.addEventListener('onload', function () {
		// 页面全部资源加载完才会执行，包括图片，视频
	})
	
	document.addEventListener('DOMContentLoaded', functioin() {
		// DOM渲染完即可执行，此时图片、视频可能没有加载完
	})
	```

## 性能优化
#### 原则
* 多使用内存、缓存或者其他方法
* 减少CPU计算、较少网络

#### 从那几方面入手
* 加载页面和静态资源

	1. 静态资源的压缩合并
	2. 静态资源的缓存(静态资源文件加版本号) --名字不变不用请求，名字变了重新请求加载
	3. 使用CDN让资源加载更快（不同区域的网络优化）-- bootCDN
	4. 使用SSR后端渲染，数据直接输出到HTML中
* 页面渲染

	1. css放head里，js放body底部
	2. 懒加载（图片懒加载、下载加载更多）
	
		```
		<img id="img1" src="preview.png" data-realsrc="abc.png" />
		<script>
			var img1 = document.getElementById("img1")
			img1.src = img1.getAttribute("data-realsrc")
		</script>
		```
	
	3. 减少DOM查询，对DOM查询做缓存

		```
		// 未缓存DOM查询
		var i = 0
		for(i = 0; i < document.getElementByTagName('p').length; i++) {
			
		}
		
		// 缓存了DOM查询
		var pList = document.getElementByTagName('p')
		var i
		for(i = 0; i < pList.length; i++) {
		
		}
	```
	
	4. 减少DOM操作，多个操作尽量合并在一起执行
	
		```
		var listNode = document.getElementById('list')
		
		// 要插入10个li标签
		var frag = document.createDocumentFragment();
		var x, li;
		for(x = 0; x < 10; x++) {
			li = document.createElement('li')
			li.innerHTML = "list item " + x;
			frag.appendChild = li; //不会触发DOM操作
		}
		listNode.appendChild = frag;
		```
	
	5. 事件节流（事件代理）

		```
		var textarea = document.getElementById('text')
		var timeoutId;
		textarea.addEventListener('keyup', function() {
			if (timeoutId) {
				clearTimeout(timeoutId)
			}
			timeoutId.setTimeout(function() {
				// 触发change事件
			}, 100)
		})
		```
	
	6. 尽早执行操作（如DOMContentLoaded）

## 面试技巧
#### 简历
* 重点突出项目经历和解决方案，技术栈，核心的解决
* 把个人博客放在简历中，并定期维护更新博客
* 把个人的开源项目放在简历中，并维护开源项目
* 简历千万不要造假，并保持能力和经历上的真实性

#### 过程
* 如何看待加班？加班就像借钱，救急不救穷，加班作为常态不太好，业余时间用来学习，交友等等
* 千万不要挑战面试官，不要反考面试官
* 学会给面试官惊喜，但不要太多
* 遇到不会回答的问题，说出你知道的也可以
* 谈谈你的缺点 -- 说一下最近学什么东西就可以