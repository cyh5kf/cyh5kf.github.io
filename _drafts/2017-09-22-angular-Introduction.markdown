---
layout: post
title: angular系列一（简介）
date: 2017-09-22 12:00:00.000000000 +08:00
---

最近因为项目需要用到angular框架，所以需要学习angular框架，故此找了一些教程，并记录下相关知识点。

第一篇博客讲解angular简介，以及angular程序架构

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


## angular程序架构
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
循环

```
*ngFor="let a of as"
```






