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

### 内存控制
V8限制堆内存的大小，表层原因为V8最初为浏览器设计，不太可能遇到用大量内存的场景。深层原因是V8的垃圾回收机制的限制。

在默认设置下，如果一直分配内存，在64位系统和32位系统下会分别只能使用余额1.4GB和约0.7GB的大小。

#### 高效使用内存
变量的主动释放

由于全局作用域需要到进程退出才能释放，讲导致引用的对象常驻内存。如需释放，可以通过delete删除引用关系，或者将变量重新复制。

```
global.foo = "I am global object";
console.log(global.foo);  // "I ..."
delete global.foo;
// 或重新赋值
global.foo = undefined; // or null
console.log(global.foo) // undefined
```
两个种方式具有同样的效果，但在V8中通过赋值方式解除引用更好。

#### 闭包
在JavaScript中，实现外部作用域访问内部作用域变量的方法叫做闭包。函数可以作为参数或者返回值。

```
var bar = function() {
	var local = "局部变量";
	return function() {
		return local;
	}
}
var baz = bar(); // 返回一个函数
console.log(baz()); // 得到局部变量
```
闭包的问题在于，一旦有变量引用这个中间函数，这个中间函数将不会被释放，同时也会使原始的作用域不会得到释放，作用域中产生的内存占用也不会得到释放。除非不再有引用，才会逐步释放。

在js执行中，无法立即回收的内存有闭包和全局变量引用这两种情况。

#### 内存指标
进入node执行环境，调用process.memoryUsage()可以看到node进程的内存占用情况。

os.totalmem()和os.freemem()分别查看系统的总内存和限制内存。

#### 内存泄漏
* 缓存
* 队列消费不及时
* 作用域未释放

#### 大内存应用
由于V8的内存限制，无法通过fs.readFile()和fs.writerFile()直接进行大文件操作，而改用fs.createrReadStream()和fs.createrWriterStream()方法通过流的方式实现对大文件的操作。

```
var reader = fs.createReadStream('in.txt');
var writer = fs.createrWriteStream('out.txt');
reader.on('data', function(chunk) {
	writer.writer(chunk);
})
reader.on('end', function() {
	wirter.end();
}
```
可读流提供管道方法pipe()，封装了data事件和写入操作。

```
var reader = fs.createReadStream('in.txt');
var writer = fs.createrWriteStream('out.txt');
reader.pipe(writer);
```

### 理解Buffer
#### Buffer结构
Buffer是个向Array的对象，但它主要用于操作字节

Buffer是一个js与C++结合的模块，性能相关部分用C++实现，非性能部分用js实现。

Buffer在进程启动时就已经加载了，放在全局对象上，所以在使用Buffer时，无须通过require()即可直接使用。

Buffer对象类似于数组，它的元素为16进制的两位数，即0到255的数值。

```
var str = "深入浅出node.js";
var buf = new Buffer(str, 'utf-8');
console.log(buf);
// => <Buffer e6 b7 85 a5...>
```
可以访问length属性得到长度

```
var buf = new Buffer(100);
console.log(buf.length); // => 100
```
可以通过下标访问元素,元素值是一个0-255的随机值

```
console.log(buf[10]);
```
```
buf[10] = 100;
console.log(buf[10]); // 100
buf[20] = -100;
console.log(buf[20]); // 156
buf[21] = 300;
console.log(buf[21]); // 44
buf[22] = 3.1415;
console.log(buf[22]); // 3
```
给元素的赋值小于0，就将该值逐次加256，直到得到一个0到255之间的整数。如果得到的数值大于255，就逐次见256，直到得到0~255区间内的数值。如果是小数，舍弃小数部分，只保留整数部分。

#### Buffer的转换
* 字符串转Buffer

```
new Buffer(str, [encoding]);
```

* Buffer转字符串

```
buf.toString([encoding], [start], [end]);
```

#### Buffer的拼接

```
var fs = require('fs');
var rs = fs.createReadStream('test.md');
var data = '';
rs.on("data", function (chunk) {
	data += chunk;
}
rs.on("end", function () {
	console.log(data)
}
```
data事件中获取的chunk对象即是Buffer对象。但是这种方法如果存在中文字符，得到的结果可能会产生乱码。

#### 正确拼接Buffer
将多个小Buffer对象拼接成一个Buffer对象，然后通过iconv-lite模块来转码

```
var chunks = [];
var size = 0;
res.on('data', function() {
	chunks.push(chunk);
	size += chunk.length;
}
res.on('end', function() {
	var buf = Buffer.concat(chunks, size);
	var str = iconv.decode(buf, 'utf8');
	console.log(str);
});
```

在网络中传输字符串，都需要转换成Buffer，以进行二进制数据传输，可以提高传输性能。


### 网络编程
node提供了net、dgram、http、https这四个模块，分别处理TCP、UDP、HTTP、HTTP。

#### TCP
TCP属于传输层协议，TCP是面向连接的协议，其显著的特征是在传输之前需要3次握手形成绘画。客户端——服务器端，请求连接-》响应—》开始传输。

构建一个tcp服务

```
// client.js
var net = require('net');
var client = net.connect({port:8124}, function() {
	console.log('client connected');
	client.write('world!\r\n');
})

client.on('data', function(data) {
	console.log(data.toString());
	client.end();
})

client.on('end', function() {
	console.log('client disconnected');
})

node client.js
```

#### UDP服务
UDP又称用户数据包协议，与TCP一样同属于网络传输层。它无需连接，资源消耗低，处理快速灵活，所以常常应用在那种偶尔丢一两个数据包也不会产生重大影响的场景，比如音频、视频等。

创建一个UDP套接字

```
var dgram = require('dgram');
var socket = dgram.createSocket("udp4");
```
创建UDP服务器端示例：

```
var dgram = require('dgram");
var server = dgram.sreateSocket("udp4");
server.on("message", function(msg, rinfo) {
	console.log("server got: " + msg + " from " + rinfo.address + ":" + rinfo.port);
});

server.on("listening", function() {
	var address = server.address();
	console.log("server listening " + address.address + ":" + address.port);
});
server.bind(41234);
```

创建UDP客户端

```
// client.js
var dgram = require('dgram');
var message = new Buffer("深入浅出node.js");
var client = dgram.createSocket("udp4");
client.send(message, 0, message.length, 41234, "localhost", function(err, bytes) {
	client.close();
})

执行命令 node server.js
```

#### 构建HTTP服务
实现一个http服务器：

```
var http = require('http');
http.createServer(function (req,res) {
	res.writeHead(200, {'Content-Type': 'text/plain'});
	res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```

HTTP构建在TCP之上，属于应用层协议。在HTTP的两端是服务器和浏览器，即著名的B/S模式，web即是HTTP的应用。