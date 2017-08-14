---
layout: post
title: 深入浅出nodejs读书笔记
date: 2017-08-13 14:03:00.000000000 +08:00
---

### nodejs的特点
* 异步I/O
* 事件与回调函数
* 单线程
* 跨平台

### nodejs模块机制
#### nodejs采用CommonJS模块规范

1.模块引用

```
var math = require('math');
```
2.模块定义

// math.js

```
exports.add = function() {
	var sum = 0,
		i = 0,
		args = arguments,
		l = args.length;
	while (i < 1) {
		sum += args[i];
	}
	return sum;
}
```
// program.js

```
var math = require('math');
exports.increment = function (val) {
	return math.add(val, 1);
}
```

#### AMD规范

AMD规范是CommonJS模块规范的一种延伸，特点是引用前置，其对应的库为requirejs。它的模块定义如下：

```
define(id?, dependencies?, factory)
```
{% highlight ruby %}
define(function() {
	var exports = {};
	exports.sayHello = function() {
		alert('Hello form module: ' + module.id');
	};
	return exports;
}
{% endhighlight %}

#### CMD规范

CMD规范由国内的玉伯提出，对用库为seajs。与AMD规范的区别是，AMD需要在申明模块的时候指定所有的依赖，通过形参传递依赖到模块内容中：

```
define(['dep1', 'dep2'], function(dep1, dep2) {
	return function() {};
});
```
与AMD模块规范相比，CMD模块更接近于Node对CommonJS规范的定义：

```
define(factory);
```
在依赖部分，CMD支持动态引入，示例如下：

```
define(function(require, exports, module) {
	// The module code goes here
});
```
require、 exports和module通过形参传递给模块，在需要依赖模块时，随时调用require()引入即可。

### 异步I/O
#### 现实中的异步I/O是如何实现的
通过部分线程进行阻塞I/O或者非阻塞I/O加轮询技术来完成数据的获取，让一个线程进行计算处理，通过线程之间的通信将I/O得到的数据就进行传递，这就轻松实现了异步I/O（尽管它是模拟的）。

Windows下的异步I/O方案是IOCP，调用异步方法，等待I/O完成之后的通知，执行回调，用户无需考虑轮询。但是它的内部仍然是线程池原理，不同之处在于这些线程池由系统内核接手管理。

Node是单线程的，这里的单线程仅仅只是JavaScript执行在单线程罢了。在Node中，无论是*nix还是Windows平台，内部完成I/O任务的另有线程池。

事实上，在Node中，除了JavaScript是单线程外，Node自身其实是多线程的，只是I/O线程使用的CPU较少。另一个需要重视的观点是，除了用户代码无法并行执行外，所有的I/O（磁盘I/O和网络I/O等）则是可以并行起来的。

### 异步编程
#### 难点
1.异常处理

2.函数嵌套过深

3.阻塞代码

4.多线程编程

5.异步转同步

#### 异步编程解决方案
1.事件发布/订阅模式

```
// 订阅
emitter.on("event", function(message) {
	console.log(message);
})
// 发布
emitter.emit('event1', "I am message");
```
2.Promise/Deferred模式
##### Promises/A
Promise操作只有三种状态：未完成态、完成态和失败态，只会出现从未完成态向完成态或失败态转化，不能逆反。一个Promise对象具有then()方法。

3.流程控制库

尾触发方法是需要手工调用才能持续执行后续调用，常见的是next。

`app.stack = []` stack属性是这个服务器内部维护的中间件队列

以下为use()方法的重要部分

```
app.use = function(route, fn) {
	this.stack.push({ route: route, handle: fn});
	return this;
}
```
监听函数实现：

```
app.listen = function() {
	var server = http.createServer(this);
	return server.listen.apply(server, arguments)；
}
```
```
app.handle = function(req, res, out) {
	next();
}
```
next()方法原理就是取出队列中的中间件并执行，同时传入当前方法以实现递归调用,达到持续出发的目的。

```
function next(err) {
	layer = stack[index++];
	
	layer.handle(req, res, next);
}
```
另外一个知名的流程控制模块async，通过npm install安装

async.series()用于处理异步的串行执行

```
async.series([
	function(callback) {
		fs.readFile();
	},
	function(callback) {
		fs.readFile();
	}
], function(err, results) {
	
})
```

async.parallel()用于异步的并行执行

```
async.parallel([
	function(callback) {
		fs.readFile();
	},
	function(callback) {
		fs.readFile();
	}
], function(err, results) {
	
})
```

async.waterfall()用于异步调用的依赖处理

```
async.waterfall([
	function(callback) {
		fs.readFile('file1.txt', 'utf-8', function(err, content) {
			callback(err, content);
		});
	},
	function(arg1, callback) {
		// arg1 => file2.txt
		fs.readFile(arg1, 'utf-8', function(err, content) {
			callback(err, content);
		});
], function(err, results) {
	// results => file3.txt

})
```

async.auto()用于自动依赖处理

另一个流程控制库是Step，通过npm install step安装

#### 异步并发控制
bagpipe模块

* 通过一个队列来控制并发量
* 如果当前活跃（指调用发起但未执行回调）的异步调用量小于限定值，从队列中取出执行
* 如果活跃调用达到了限定值，调用暂时存放在队列中
* 每个异步调用结束，从队列中取出新的异步调用执行



