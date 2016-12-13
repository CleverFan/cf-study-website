---
title: linux安装tomcat
type: linux
order: 4
---

## linux安装tomcat

> 请先看上一篇，linux安装jdk。因为tomcat需要jdk的支持。


### 如何安装

新建一个文件夹用来存放tomcat
```js
//新建文件夹
mkdir /usr/tomcat
//切换到该文件夹下
cd /usr/tomcat
```

//使用wget，下载tomcat包
```
wget -c http://mirrors.hust.edu.cn/apache/tomcat/tomcat-8/v8.5.9/bin/apache-tomcat-8.5.9.tar.gz
```

下载文笔后，可以使用`ls` 命令查看：

> ![这里写图片描述](http://img.blog.csdn.net/20161213164808670?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE2NTU5NjU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

解压下载的文件
```js
tar -zxvf apache-tomcat-8.5.9.tar.gz  
```
> ![这里写图片描述](http://img.blog.csdn.net/20161213164853342?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE2NTU5NjU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 

解压成功后，你的tomcat就安装好了。

启动tomcat
```js
//切换到bin目录下
cd apache-tomcat-8.5.9/bin 
//运行启动脚本
./startup.sh 
```
> ![这里写图片描述](http://img.blog.csdn.net/20161213165038793?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE2NTU5NjU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


启动成功。

### 如果你有防火墙

那么请这样做：

编辑防火墙配置文件

```js
vi /etc/sysconfig/iptables
```
按“i”进入输入模式，添加以下内容

```js
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT

```

按esc,输入“:wq”保存并退出。

重新启动防火墙

```js
service iptables restart
```


### 测试

在浏览器里输入 ip:8080

> ![这里写图片描述](http://img.blog.csdn.net/20161213165339891?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE2NTU5NjU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


关闭服务器
```js
./shutdown.sh
```

tomcat大概就是这样了，其他的操作你用过tomcat自然就懂了。

### 其他

默认下载的包是tomcat8的，如果你需要下载其他版本，请将wget命令后的地址换成你需要的版本的下载地址。下载地址的获取方式：

> ![这里写图片描述](http://img.blog.csdn.net/20161213165522876?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzE2NTU5NjU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)