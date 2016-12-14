---
title: 使用express搭建nodejs项目
type: nodejs
order: 2
---

最近对nodejs比较感兴趣，所以打算学习一下。但是在学习过程中遇到了一些麻烦，现在将这个过程与大家分享一下。

## 环境

 - 系统 ubutu16.04 （win10下的虚拟机）
 - nodejs版本 4.4.7
 - npm版本 2.15.8

> nodejs  github主页：https://github.com/nodejs/node
> nodejs  官网：https://nodejs.org
> nodejs 中文社区：https://cnodejs.org
> express github主页：https://github.com/expressjs/express

## 第一步，安装nodejs

**windows**请直接到上诉官网中下载安装即可，现在安装nodejs会自动将npm也安装好。

**Ubuntu系统**：
首先确保系统安装来python,gcc,g++,如果没有则安装： 
（一般Ubuntu都已默认安装）

```js
$ sudo apt-get install python 

$ sudo apt-get install build-essential 

$ sudo apt-get install gcc 

$ sudo apt-get install g++ 
```

确保都安装了之后，从nodeJS官网下载最新源代码包：node-v4.4.7.tar.gz

解压：

```js
$ tar -zxf node-v4.4.7.tar.gz 

$ cd node-v4.4.7
```

默认安装： 

```js
$ ./configure 

$ make 

$ sudo make install 
```

选择目录方式安装： 

```js
$ ./configure –prefix=/usr/node 

$ make -j 5 #5=CPU核数+1 

$ sudo make install
```

> 当然，你也可以通过linux包管理器安装：
> `apt get install nodejs -g`
> `apt get install npm -g`
> 但是个人不建议这么做，因为有时候会莫名其妙的出问题，还是直接编译源码比较实在（编译过程需要一些时间）。

接下来通过查看版本确认已经安装成功：

```js
$ node -v
```

```js
$ npm -v
```

## 第二步，安装express

**Windows**：打开Node.js command prompt，输入：
	

```js
npm install express -g
```

> -g是全局安装，在任何地方打开命令行都可以使用相应命令

**Ubuntu** 打开终端，输入：
	

```js
$ sudo npm install express -g
```

> 请务必使用管理员权限安装，否则会安装失败

```js
$ sudo npm install -g express-generator@4  
```

 

>express4.0之后把创建一个APP的功能分离出来为express-generator，所以必须得安装它，否则没法正常使用express

通过输入‘express -v’确认安装成功。（有些情况无法查看版本，但是可以正常使用）

## 使用express建一个demo

  
 1. 打开一个用来放源码的目录，输入`$ express nodetest -e`  

> 新建一个名为nodetest的项目，使用ejs作为模板。 ejs （Embedded JavaScript） 是一个标签替换引擎，其语法与 ASP、PHP 相似，易于学习，目前被广泛应用。Express 默认提供的引擎是 jade，它颠覆了传统的模板引擎，制定了一套完整的语法用来生成 HTML 的每个标签结构，功能强大但不易学习。Express 在初始化一个项目的时候需要指定模板引擎，默认支持Jade和ejs。

 2. `$ cd nodetest && npm install`
 

> 进去项目文件夹，并安装所需要的依赖。无参数的 npm install 的功能就是 检查当前目录下的 package.json，并自动安装所有指定的依赖。 

 3. `$ npm start`
 

> 启动项目。接下来，打开浏览器，输入地址 http://localhost:3000，你就可以看到一个简单的 Welcome to Express 页面了。


> express以前的版本，启动项目的命令是
> `\$ node app.js`
> 但是，使用最新版的这样是无法启动的，正确的启动命令是
> `$ npm start`

市面上讲nodejs的书籍并不多，而且node的版本更新太快，好多书上的例子用最新版的node已经无法运行了，所以，不要觉得书上的都是正确的，遇到问题多google。



> 附nodejs教程，中文文档等
> http://download.csdn.net/detail/qq_31655965/9603069
> http://download.csdn.net/detail/qq_31655965/9603064

     