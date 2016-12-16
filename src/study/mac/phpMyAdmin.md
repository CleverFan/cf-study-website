---
title: mac下安装phpMyAdmin
type: mac
order: 2
---

## 使用php环境

为什么要用使用这个词呢。因为从 OS X 10.0.0 版本开始，PHP 作为 Mac 机的标准配置被提供，所以，你不需要额外安装php，只需要简单的配置一下就好了。

1.找到并打开Apache的配置文件。默认情况下，这个配置文件的位置是： 
```
/private/etc/apache2/httpd.conf
```

2.在这个配置文件中，找到
```
LoadModule php5_module libexec/httpd/libphp5.so
```

默认情况下它是被注释的，我们把它前面的注释取消。

> ![取消注释](http://upload-images.jianshu.io/upload_images/4047674-b59a46f9ce4d8760.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 


3.在终端中输入

```
sudo apachectl restart

```
 重启Apache服务器。

4.在浏览器中输入
```
localhost

```
进行测试

> ![成功](http://upload-images.jianshu.io/upload_images/4047674-67b6bd6f497c4b11.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

这样就证明成功了。

5.在配置文件中，找到这句话：

> ![路径](http://upload-images.jianshu.io/upload_images/4047674-3425c0de667f0c86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

这里的路径就是服务器项目的目录，我们只要把项目放到这个目录下就可以运行了。

## 安装phpMyAdmin

1.  到官网下载最新版本的包。

> https://www.phpmyadmin.net/


点击右上角的download下载即可。

2.下载到本地后，解压到我们刚刚记录的那个目录下（/library、WebServer/Documents）。

3.解压后的文件名很长（phpmy-xxxx-xxx)，不利于访问（因为访问的路径中有这个文件的名字），我们可以改成自己喜欢的名字，暂定改成phpMyAdmin。

4.复制config.example.inc.php 保存为：config.inc.php 

phpmyadmin的运行需要config.inc.php 配置文件，但是官方只给我们提供了config.example.inc.php作为一个demo。所以或者修改文件名，或者复制一份然后改名。推荐复制一份，这样demo还可以随时浏览。

5.修改配置文件config.inc.php。


```

 */
$cfg['blowfish_secret'] = 'chengfan'; /* YOU MUST FILL IN THIS FOR COOKIE AUTH! */

/**
 * Servers configuration
 */
$i = 0;

/**
 * First server
 */
$i++;
/* Authentication type */
/* Authentication type */
$cfg['Servers'][$i]['user'] = 'root'; //mysql username here
$cfg['Servers'][$i]['password'] = 'root'; //mysql password here
$cfg['Servers'][$i]['auth_type'] = 'cookie';
/* Server parameters */
$cfg['Servers'][$i]['host'] = '127.0.0.1';
$cfg['Servers'][$i]['connect_type'] = 'tcp';
$cfg['Servers'][$i]['compress'] = false;
$cfg['Servers'][$i]['AllowNoPassword'] = false;
```
只修改这些就可以使用了。

注意，host最好改成127.0.0.1，不要使用localhost，否则肯能登录不上。

6.保存以后，浏览器访问localhost/phpMyAdmin 即可使用。（localhost后面的就是你phpmyadmin的文件名）

