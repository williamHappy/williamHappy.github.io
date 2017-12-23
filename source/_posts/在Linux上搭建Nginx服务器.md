---
title: 在Linux上搭建Nginx服务器
date: 2017-06-11
top: 15
categories: [后端]
tags: [Linux,Nginx]
description: "项目开发简便易行，提高效率，才是关键所在！"
---
<!--more-->


[toc]

同样是分布式系统中，需要使用nginx服务器，关于nginx的相关概念知识，百度google吧，就不多说了，主要说一下自己Linxu上搭建nginx服务器是遇到的问题，并且在此项目中，是使用虚拟机搭建一个专门的服务器来存放图片，在此服务器上安装一个nginx来提供http服务，安装一个ftp服务器来提供图片的上传服务。

### 一. 环境准备

我自己的环境（可以自己选择版本）：
1. 安装了CentOS7的VM虚拟机（如何安装，参考[Linux学习](http://willhappy.cn/2017/01/08/Linux%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0%EF%BC%88%E5%88%9D%E7%BA%A7%EF%BC%89/)  ）
2. Xshell5安全终端模拟软件（下载地址：http://www.netsarang.com/download/free_license.html ）

*在这多说一点，关于xshell连接CentOS时的问题，如果你的xshell可以连接到Linux，那直接跳到第三节，没有连接好，看第二节，当然你也可以直接登录centos，在终端进行操作，使用xshell主要是方便可以再Windows界面下访问不同系统下的服务器，从而较好的达到远端控制终端的目的。*

### 二. Xshell连接CentOS

下载好xshell，安装完成后，启动，首先，要保证centos，可以连接上网。不能上网的话，参考博客：http://www.linuxidc.com/Linux/2013-05/83803.htm
然后就是使用xshell连接centos，参考：http://jingyan.baidu.com/article/36d6ed1f7520991bcf4883e6.html

关于Xshell连接Centos就说这么多了。以后多使用，应该就熟练了。

### 三. 安装Nginx

1. nginx安装环境
nginx是C语言开发，建议在linux上运行，本教程使用Centos6.5作为安装环境。
- gcc
	安装nginx需要先将官网下载的源码进行编译，编译依赖gcc环境，如果没有gcc环境，需要安装gcc：
```
yum install gcc-c++ 
```
- PCRE
	PCRE(Perl Compatible Regular Expressions)是一个Perl库，包括 perl 兼容的正则表达式库。nginx的http模块使用pcre来解析正则表达式，所以需要在linux上安装pcre库。
```
yum install -y pcre pcre-devel
```

注：pcre-devel是使用pcre开发的一个二次开发库。nginx也需要此库。
- zlib
	zlib库提供了很多种压缩和解压缩的方式，nginx使用zlib对http包的内容进行gzip，所以需要在linux上安装zlib库。
```
yum install -y zlib zlib-devel
```

-	openssl
	OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。
	nginx不仅支持http协议，还支持https（即在ssl协议上传输http），所以需要在linux安装openssl库。
```
yum install -y openssl openssl-devel
```


2. 编译安装

- 首先进入/usr/local
```
cd /usr/local
```

- 从官网下载nginx（版本自己选择）
```
wget http://nginx.org/download/nginx-1.9.8.tar.gz
```

- 解压nginx压缩包
```
tar -zxvf nginx-1.9.8.tar.gz
```

- 会产生一个nginx-1.9.8目录，进入这个目录
```
cd nginx-1.9.8
```

- 接下来安装，使用--prefix参数指定nginx安装目录
- ./configure --help查询详细参数
```
./configure \
--prefix=/usr/local/nginx \
--pid-path=/var/run/nginx/nginx.pid \
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module \
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--http-scgi-temp-path=/var/temp/nginx/scgi

```
<p style="color:red;">注意：上边将临时文件目录指定为/var/temp/nginx，需要在/var下创建temp及nginx目录</p>

- 编译安装
```
make
make  install

```
安装成功查看安装目录：
![nginxDir](https://raw.githubusercontent.com/williamHappy/FileRepo/master/hexo/20170113/Java009/img/nginxDir.png)

### 三. 启动Nginx

1. 进入nginx安装目录，启动nginx
```
cd /usr/local/nginx/sbin/
./nginx
```
查看nginx进程：
![nginxProcess](https://raw.githubusercontent.com/williamHappy/FileRepo/master/hexo/20170113/Java009/img/nginxProcess.png)

2. 停止nginx
方式1，快速停止：
```
cd /usr/local/nginx/sbin
./nginx -s stop
```
此方式相当于先查出nginx进程id再使用kill命令强制杀掉进程。

方式2，完整停止(建议使用)：
```
cd /usr/local/nginx/sbin
./nginx -s quit
```
此方式停止步骤是待nginx进程处理任务完毕进行停止。

3. 重启nginx

方式1，先停止再启动（建议使用）：
对nginx进行重启相当于先停止nginx再启动nginx，即先执行停止命令再执行启动命令。
如下：
```
./nginx -s quit
./nginx
```

方式2，重新加载配置文件：
当nginx的配置文件nginx.conf修改后，要想让配置生效需要重启nginx，使用-s reload不用先停止nginx再启动nginx即可将配置信息在nginx中生效，如下：
```
./nginx -s reload
```

### 四. 测试
nginx安装成功，启动nginx，即可访问虚拟机上的nginx：
![nginxRunSuccess](https://raw.githubusercontent.com/williamHappy/FileRepo/master/hexo/20170113/Java009/img/nginxRunSuccess.png)
到这说明nginx上安装成功。

