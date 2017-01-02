
---
title: 使用vps搭建vpn！
type: linux
order: 5
---

> 作为一个程序员，不科学上网简直没法生存下去，从去年开始国家封了一堆vpn，免费科学上网似乎越来越难了。不过，天下没有免费的午餐，想白吃白喝一辈子是不行的，所以，有的时候，还是花点钱比较好。

<!-- more -->

## **简单的科学上网**

### **免费的**
讲道理，现在免费的真的不多了，shadowsocks，goagent都被搞了。

> github源码被要求清空

>![这里写图片描述](http://img.blog.csdn.net/20161124104051542)

现在还可以免费使用的，我知道并且用过的：

 - lantern
	 > 这个大概是唯一存活的开源服务了吧，免费版每个月800M流量，浏览网页速度还可以，看视频没试过，我不怎么看。 
	 
	 > 唯一官方下载地址：
	 https://github.com/getlantern/forum/issues/833。
	 别随便百度一个就下！！！
<br>
> ![这里写图片描述](http://img.blog.csdn.net/20161124104429840)

 - 天行浏览器
 > 只支持windows，是一个科学上网浏览器，在chrome的基础上开发的，什都不需要做，打开浏览器就可以科学上网。不过只能安装在c盘，你懂得，免费的总会有安全问题，不建议使用。

 - 各大收费vpn的免费服务
 
 >这个就多了，基本上每个收费的vpn都会有免费的方式。比如一天一小时，或者流量限制，不想花钱，又想科学上网，就多下几个vpn，换着用，但是感觉很麻烦，而且，免费意味着风险。
 
###**要钱的**

 - 各种vpn
  > 这个看着选吧，我也没怎么用过，只用过天行vpn的免费时段。

 - lantern专业版 
  > 一年180，两年320，比一般vpn便宜不少，可以同时登陆三个账户。 


##**采用国外vps搭建vpn**
这才是本文的重点
 > 好处，自己搭建的，没什么安全问题，而且更划算（我用digitalocean的服务器，一个月5$，大概35元左右，你除了搭建一个vpn，还相当于获得了一个服务器，国内服务器的价格可比这个高了不少，比如腾讯云最低配置的服务器比do最低配置低，但是一个月65。你懂得。da当然，如果你的预算不够，那你可以选择搬瓦工的服务器，搬瓦工最便宜的好像是2.99$一个月。
 > 如果你是学生就更好了，github学生包里有DO 的50\$ 代金券，相当于你可以免费用10个月，何乐而不为呢）

### **如果你是学生**

1 。 注册一个github账号

> https://github.com

2。获取github学生包

> https://education.github.com/
> 需要提供自己是学生的信息，比如.edu结尾的邮箱

3。注册digitalocean，使用学生包提供的code

> 注册地址 : https://m.do.co/c/732fd264c055 【请先科学上网】
> 注意：注册的时候需要先充5\$(至少），最后，你的账户里会有55\$:
> 
> 使用我上面提供的链接注册，会获得10$，也就是说，你会有65$。

> ![这里写图片描述](http://img.blog.csdn.net/20161124110151568) 

### **你不是学生**

那你就直接注册充钱好了，如果你觉得DO有点贵，可以使用搬瓦工（https://bandwagonhost.com)，搬瓦工有2.99\$每月的服务器，也够用了。我使用DO大部分原因是因为github提供的50\$。。。

>**请先科学上网，然后再访问DO或者搬瓦工**

## **如何搭建**

### **创建一个服务器，推荐使用CentOS 7**

> Droplets -> create droplet -> choose CentOS 7.2x64 ->选择最便宜的版本 ->选旧金山 -> private networking -> new ssh key ->add sshkey ->可以改个名字啥的

-----------

> sshkey不创建也可以，但是为了安全还是创建比较好，当然，以后创建也是可以的。

-------------

> 创建ssh key （mac）：
- 打开终端 
- 输入`ssh-keygen -t rsa` 回车
- 可以输入passphrase，也可以不输入
- 地址默认就好，然后就生成了
- `cat /Users/chengfan/.ssh/id_rsa.pub`
- 把显示出来的秘钥复制到DO create ssh key 中key面板中
- name  随便写
- 搞定
	
### **配置你的vps服务器**

 - 使用ssh连接
  >mac&linux : 终端（或者使用zoc7）输入  **ssh root@你的ip地址**，如果你刚刚创建ssh key的时候设置了密码，这个时候需要输入密码，否则直接连上。这个时候，你的终端就相当于你vps的终端，可以做任何你想做的事情。
 
  >win : 推荐使用putty，请自行研究
 
 - 配置防火墙
> 暂时不需要，就不麻烦了

 - 安装必要组件

```js 
$ yum install m2crypto python-setuptools
$ easy_install pip
$ pip install shadowsocks
```

 - 配置服务器参数

```js
$ vi  /etc/shadowsocks.json

//按 i 进入输入模式  输入以下内容：

{
    "server":"0.0.0.0",//服务器ip地址
    "server_port":443,//端口号，可以改
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",//这里改成你自己的密码，客户端需要
    "timeout":300,
    "method":"aes-256-cfb",//加密方式
    "fast_open": false,
    "workers": 1
}

// ESC -> :WQ  保存并退出
```

 - 运行命令，启动 Shadowsocks 服务

```js
$ ssserver -c /etc/shadowsocks.json //启动服务

$ ctrl + c //停止服务
```


### **关键一步-----本地科学上网**

高了那么多，这一步才是最重要的！

- 1 下载shadowsockes客户端

> https://github.com/shadowsocks/shadowsocks/wiki/Ports-and-Clients
> 包括各个平台，自行下载

- 2 配置代理服务器（以mac为例）
>![这里写图片描述](http://img.blog.csdn.net/20161124130151739)

- 添加一个服务器
>![这里写图片描述](http://img.blog.csdn.net/20161124130414133)

- 现在就可以访问被墙的网站了
>![这里写图片描述](http://img.blog.csdn.net/20161124130710153)

- 其他平台同理





至此shadowsocks搭建完成，shadowsocks已经可以使用，如果你没有过高的要求，下面的步骤可以省略，下面是后台运行 Shadowsocks 的步骤。

### **shadowsocks后台运行**

 - 安装 supervisor 实现后台运行

```js
$ easy_install supervisor
```

 - 然后创建配置文件

```js
$ echo_supervisord_conf > /etc/supervisord.conf

```

 - 修改配置文件

```js
$ vi /etc/supervisord.conf

//在文件末尾添加

[program:ssserver]
command = ssserver -c /etc/shadowsocks.json
autostart=true
autorestart=true
startsecs=3
```

 - 设置 supervisord 开机启动，编辑启动文件

```js
$ vi /etc/rc.local
//在末尾另起一行添加

$ supervisord  //开启

$ supervisorctl stop ssserver  //关闭
$ supervisorctl restart ssserver //重启
```

 - 为 rc.local 添加执行权限

```js
$ chmod +x /etc/rc.local
```

 - 运用 supervisord 控制 Shadowsocks 开机自启和后台运行

```js
$ supervisored
```
大概是这样的：

> ![这里写图片描述](http://img.blog.csdn.net/20161124115324368)

 - 现在你可以随心所欲的科学上网了。


###**使用 TcpSpeed 来加速科学上网**

如果你觉得速度不够快，那么可以使用TcpSpeed（曾经的FinalSpeed）加速，不过这个不是免费的，需要激活码，现在是159一个，永久激活。

由于我现在穷，就先不搞了，丢个链接：
>http://www.tcpspeed.com/active.html

## **后记**

- 如果你不看视频，不需要经常科学上网，那么一个lantern免费版就够用了。

- 如果你喜欢科学上网浏览器并且不怕危险，就是用天行浏览器（我也不知道有没有危险）

- 如果你不差钱，可以买vpn或者lantern专业版

- 如果你是一个程序员，并且经常和服务器打交道，我建议你买一个服务器，最低配的就行，比如DO，一个月35左右的价格挺划算的，比国内的便宜不少，不仅可以搭一个自己的网站，放一些demo，而且还可以搭一个vpn。如果你觉得速度慢，就买一个TcpSpeed激活码，永久激活也蛮合适的（可看YouTube超清电影）。

<hr>
顺便说一下，我在学校用学校的网各种慢，

github慢，freecodecamp慢，stackoverflow慢，google慢。。。

用公司的网各种快，什么都快。。

等我去研究一下这是为啥


<hr>

博客同步更新在：http://blog.csdn.net/qq_31655965/article/details/53318730
