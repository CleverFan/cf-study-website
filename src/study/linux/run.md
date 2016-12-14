---
title: nodejs环境的搭建
type: linux
order: 2
---

## **nodejs环境的搭建**

> nodejs的安装方法有很多种，可以通过编译源码或者类似于apt-get等方式。但是由于我们日后可能需要在nodejs的不同版本之间切换，所以这里给大家提供的是用nvm来安装。

### **nvm是什么**

nvm (Node Version Manager) 是 Nodejs 版本管理器，它让我们能方便的对 Nodejs 的版本进行切换。

> 举个例子，假设，我们已经安装 nvm 了。如果，我们此时需要用 5.0 版本的 Nodejs ，但是我们本机没有装该版本，那么，我们先执行
> nvm install 5.0 来安装该版本，然后执行 nvm use 5.0， 此时用的 Nodejs 的版本即为 5.0的。以后我们切换到 5.0 版本只需执行 nvm use 5.0 即可。当然，我们可以用 nvm install 来装更多的版本。


### **安装nvm**

> nvm github地址：https://github.com/creationix/nvm

```js
$ wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.31.6/install.sh | bash


$ source ~/.bashrc


$ command -v nvm

//如果终端打印出“mvm”，证明安装成功
```

### **通过nvm安装nodejs**

在终端输入

```js
$ nvm ls-remote
```

> 这个命令可以查看当前发布的所有nodejs的版本，从0.1.14到6.4.0

我们选择安装当前使用人数最多的4.5.0版本（见官网）

```js
$ nvm install 4.5.0
```

指定 nvm 使用的 Node.js 版本

``` js
$ nvm use 4.5.0
```

预设使用 4.5.0 版本，否則每次重新連線登入，還需要重新 nvm use 一次

```js
$ nvm alias default 4.5.0
```

这样，nodejs就安装好了，可以通过node -v查看当前使用的版本。同时，npm也安装好了，版本为2.15.9