---
title: linux下安装jdk
type: linux
order: 3
---

## linux下安装jdk

### 先看一下你的系统有没有自带的jdk
`java -version` 
如果有，会输出相应的版本信息。那么把自带的删了。用下面的方式：

```js
rpm -qa | grep jdk 
rpm -e --nodeps xxxx //卸载对应jdk，其中xxxx为所要卸载的jdk名称
```

如果没有，那么我们直接安装

### 安装jdk
先找一个放置jdk的目录。假如为/usr/java/：

```js
//新建文件夹
mkdir /usr/java
//切换到这个文件夹
cd /usr/java

//下载rpm文件
wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u112-b15/jdk-8u112-linux-x64.rpm

```
这个时候就是在下载jdk文件了。大概160M，网速快的话很快就好了。
> ![这里写图片描述](http://img.blog.csdn.net/20161213162514447?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE2NTU5NjU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

可以使用`ls`命令，查看下载的文件。

然后

```js
//给文件添加执行权限
chmod +x jdk-8u112-linux-x64.rpm 
//使用rpm安装
rpm -ivh jdk-8u112-linux-x64.rpm
```
> ![这里写图片描述](http://img.blog.csdn.net/20161213162753022?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE2NTU5NjU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这样jdk就安装好了，测试一下

```js
//查看java版本
java -version
```
> ![这里写图片描述](http://img.blog.csdn.net/20161213162633401?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE2NTU5NjU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

接下来配置环境变量：

```js
//编辑配置文件
vi /etc/profile 
```
打开配置文件，按“i”进入输入模式

> 更多vi的用法，请查看我的vi使用讲解

把配置文件里export PATH 中的PATH删掉。

> ![这里写图片描述](http://img.blog.csdn.net/20161213184507918?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE2NTU5NjU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

在配置文件的最末尾输入以下内容

```
export JAVA_HOME=/usr/java/jdk1.8.0_112
export PATH=$JAVA_HOME/bin:$PATH 
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar 
```
然后按esc，输入“:wq”  按回车键，保存并退出

> ![这里写图片描述](http://img.blog.csdn.net/20161213162724975?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE2NTU5NjU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

重启配置文件：
```js
source /etc/profile
```

这个时候，你就可以在任何位置使用jdk了，测试一下。

```js
//切换到根目录
cd /  
//运行这两个java命令，看到结果证明成功
java
javac
```

### 使用yum命令安装openjdk。

openjdk与oracle的jdk有什么区别请自行研究。大部分人都是用Oracle的。但是如果你想使用openjdk，那么你可以用下面的方法安装。

```js
//列出所有的可安装包
 yum list java*
```

 
>![这里写图片描述](http://img.blog.csdn.net/20161213150432654?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE2NTU5NjU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  

```
//选择你需要的版本安装，比如：
yum install java-1.7.0-openjdk 
```

恩，安装好了

### 其他 

本文默认安装jdk1.8，如果你需要安装其他版本的jdk，请把wget命令后的下载地址换成你需要的版本地址。下载地址获取方法：

> ![这里写图片描述](http://img.blog.csdn.net/20161213170150629?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE2NTU5NjU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 

官网 ： http://www.oracle.com/technetwork/cn/java/javase/overview/index.html