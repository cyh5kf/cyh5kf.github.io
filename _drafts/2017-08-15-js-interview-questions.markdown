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

* #### 获取随机数，要求长度一致的字符串格式

* #### 写一个能遍历对象和数组的通用forEach函数