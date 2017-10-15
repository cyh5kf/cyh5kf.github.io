---
layout: post
title: angular系列一（简介）
date: 2017-09-22 12:00:00.000000000 +08:00
---

最近因为项目需要用到angular框架，所以需要学习angular框架，故此找了一些教程，并记录下相关知识点。

## 与react对比
### 速度
react采用虚拟DOM机制，速度非常快，先更新虚拟DOM，再更新实际DOM，比angular的优势在于：一是更新DOM的次数少，二是更新DOM的内容少。

angular采用新的变更检测算法，在速度上已经跟react不相上下了。

### FLUX架构
react更关注UI的组件化和数据的单向更新。

现在angular也已经具备了组件化，数据的单项更新，对ES6语法的支持。

### 服务器端渲染
预渲染引擎有助于搜索引擎优化，react和angular都支持服务器端渲染。

react是一个UI组件，通常需要和其他框架组合使用，不适合单独作为完整的框架。

angular提供的工具比react要多得多

## 与vue对比
### 简单
vue简单的api，vue学习使用难度不高，容易上手

### 灵活
vue灵活自由构建

### 性能
vue尺寸小，采用虚拟DOM，性能好

### 个人主导
团队项目要比个人项目靠谱，angular社区庞大。

### 只关注web
vue只关注web，angular大的前端平台，还可以开发native

### 服务器端渲染
angular官方支持


## 第一章节 —— 构建angular应用页面

### angular程序架构
![Angular程序架构](http://ouq0pnc4r.bkt.clouddn.com/angular-Architecture.png)


### 配置开发环境
安装node.js包

安装angular cli命令行工具
```
npm install -g @angular/cli
或者 sudo cnpm i -g @angular/cli
如果都不行使用 cnpm i -g angular-cli
```

创新新项目
```
sudo ng new demo
```

进入项目，安装npm包
```
cd demo
cnpm i
```

启动项目
```
sudo npm start
或者 ng serve
```

### 项目结构
* e2e 端到端的测试目录，自动测试
* src 应用源代码目录
* .editorconfig IDE配置文件
* .gitignore git的配置文件
* .angular-cli.json  angular命令行工具的配置文件，可以在这里添加第三方库
* karma.conf.js  karma单元测试的配置文件，执行自动化测试
* package.json  npm的配置文件
* protractor.json  自动化测试的配置文件 
* README.md  文档说明
* tslint.json 定义typeScript代码质量检查的规则
* src/app  包含应用的组件和模块
* src/assets 存空的静态资源，图片
* src/environments  环境配置
* src/index.html  整个应用的根html
* src/main.js 这个脚本执行的入口点
* src/polyfills.ts  导入必要的库，兼容老版本浏览器
* src/style.css 放应用的全局的样式
* src/test.js 自动化测试
* src/tsconfig.app.json  typeScript编译器的配置
* src/app/app.component.ts  

#### 组件结构
![Angular组件结构](http://ouq0pnc4r.bkt.clouddn.com/angular-models.png)

### angular启动过程
看 angular-cli.json
在main.ts中启动应用，并加载模块，在app.module.ts中加载完所有依赖的模块，在index.html中寻找AppModule中指定的主组件对应的css选择器app-root，找到选择器后，再用app.components.ts中制定的html模板内容替换掉app-root这个标签，在这个过程完成之前将会展现app-root中的字符串loading...

### 引入第三方库
```
npm install xxx --save
```

在angular-cli.json里 script数组添加node_modules里的模块文件，使用相对路径

在angular-cli.json里 style添加css文件，如果不起作用，可以在style.css文件内通过@import导入css文件，从而添加全局的css样式

安装模块的类型文件

```
npm install types/xxx --save-dev
// jquery最新版本会报错，可以指定版本
npm install @types/jquery@2.0.47 --save-dev
```

### cli命令行生成组件代码，并在app.module.ts中声明
```
ng g component xxx(组件名)
```

### ng指令
for循环

```
 *ngFor="let product of products"
```

### 插值表达式
在html模板里使用{{}}来插入数据

### 属性绑定
在组件控制器内定义`private imgUrl = "http://placehold.it/320x150";`

然后在html模板里通过[]绑定
```
<img [src]="imgUrl" alt="">
```

###属性绑定中的特殊绑定，样式绑定

```
[class.glyphicon-star-empty]="star"
```
class表示绑定一个class类名，当star为true时，class类名添加到标签上，为false则不添加到标签上。

###组件的输入属性，父组件数据传递给给子组件
首先在父组件模板上绑定属性

```
<app-stars [rating]="product.rating"></app-stars>
```

然后在子组件内
首先引入Input属性

```
import { Component, OnInit, Input } from '@angular/core';
```

再在控制器内定义输入属性

```
@Input()
  private rating:number = 0;
```
等于0为默认值

然后在子组件模板内使用插值表达式插值

```
<span>{{rating}}星</span>
```

## 第二章节 —— angular路由

### 路由相关对象
![Angular路由对象](http://ouq0pnc4r.bkt.clouddn.com/angular-router.png)

使用ng命令带routing的参数创建带有路由的ng项目

```
ng new router --routing
```
生成一个`app-routing.module.ts`文件，以及`AppRoutingModule`模块，并在`app.modules.ts`的imports中导入这个模块

在app-routing.module.ts中写路由配置

具体的路由放在前面，通配符路由放在后面

```
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { ProductComponent } from './product/product.component';

const routes: Routes = [
  {path: '', component: HomeComponent},
  {path: 'product', component: ProductComponent},
  {path: '**', component: Code404Component}
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

在`app.component.html`中写导航结构，通过link标签属性导航

```
<!--"/"表示配置更路由，参数是一个数组，在路由的时候可以传递参数-->
<a [routerLink]="['/']">主页</a>
<a [routerLink]="['/product']">商品详情页</a>
<!--ng事件绑定-->
<input type="button" value="商品详情" (click)="toProductDetail()">
<!-- router-outlet为路由插槽，显示组件内容 -->
<router-outlet></router-outlet>
```

另一种导航方式通过事件绑定，router方法跳转，在`app.component.ts`中

```
import { Component } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app';

  constructor(private router: Router) {

  }

  toProductDetail() {
    this.router.navigate(['product']);
  }
}
```

### 路由传递参数

* 在路由时传递参数

```
/product?id=1&name=2  => ActivatedRoute.queryParams[id]
```

* 在查询参数中传递数据

在html模板中定义路由参数

```
<a [routerLink]="['/product']" [queryParams]="{id: 1}">商品详情页</a>
```

然后在页面组件里写

```
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-product',
  templateUrl: './product.component.html',
  styleUrls: ['./product.component.css']
})
export class ProductComponent implements OnInit {

  private productId: number;

  constructor(private routerInfo: ActivatedRoute) { }

  // 组件声明周期中的钩子，在组件实例化后调用一次
  ngOnInit() {
    this.productId = this.routerInfo.snapshot.queryParams["id"];
  }

}
```

在页面模板里通过插值表达式，即可展示路由信息

```
<p>商品ID是： {{productId}}</p>
```

* 在路由路径中传递数据

在app-routing.module.ts路由配置文件中修改，添加路由参数

```
{path: 'product/:id', component: ProductComponent}
```

在app.component.html模板中通过数组添加参数

```
<a [routerLink]="['/product', 1]">商品详情页</a>
```

在product.component.ts组件中修改查询参数的方法，`snapshot`叫做参数快照

```
ngOnInit() {
    this.productId = this.routerInfo.snapshot.params["id"];
  }
```

### 在app.component.ts中修改点击跳转事件

```
toProductDetail() {
    this.router.navigate(['product', 2]);
  }
```

当从主页跳转到商品详情页时，商品详情组件被创建，ngOnInit方法被调用。而当商品详情已经被创建时，再去点击商品详情按钮，这时候ngOnInit方法不会被调用，所以路由参数信息不会改变。

这时候需要把参数快照改为参数订阅模式

```
import { ActivatedRoute, Params } from '@angular/router';

this.routerInfo.params.subscribe((params: Params) => this.productId = params["id"]);
```


### 重定向路由
在用户访问一个特定的地址时，将其重定向到另一个指定的地址

在app-routing.module.ts中修改路由配置

```
{path: '', redirectTo: '/home', pathMatch: 'full'}
```

### 子路由


