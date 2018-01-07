---
title: 安装Nginx和vsftpd的坑
date: 2017-06-13
categories: [后端]
tags: [Linux,Nginx,vsftpd]
description: "Linux还是要好好学的，要不安装个东西太累了！"

---
<!--more-->

[toc]

为了搭个图片服务器，在centos上安装配置Nginx和vsftpd服务，简直折腾了我一天的时间，最终得出结论，还是要好好学习Linux，不然真是折腾死个人！

### 一. Nginx访问问题

1. nginx安装完成之后，启动起来了，发现在物理机中访问不到，这可能的原因是Linux防火墙的问题。
解决方案：http://www.cnblogs.com/yomho/p/6074815.html
谢谢博主，我就不写了//呲牙。

2. 前一天装好了，第二天起来，再次启动，发现起不来了Nginx，错误信息如下：
![startNginxErr](https://raw.githubusercontent.com/williamHappy/FileRepo/master/hexo/20170113/Java009/img/startNginxErr.png)

实在没找到什么原因，怀疑是由于nginx停止的方式不对，因为前一天晚上因为机子卡，vm非正常关闭，导致了无法启动，果断重装了。以后再遇到就再说了，要不进行不下去了。

后来找到解决方案：
```
[root@localhost var]# cd /var/run		###先cd到/var/run目录下
	[root@localhost run]# mkdir nginx		###然后创建nginx目录
	[root@TEST sbin]# sudo ./nginx -c /usr/local/nginx/conf/nginx.conf		###重新配置nginx配置文件
	[root@TEST sbin]# ./nginx -s reload		###重新加载配置文件
	虚拟机重启，需要重新配置文件即可

```

方案二：参考
https://jingyan.baidu.com/article/f00622281858e2fbd3f0c81b.html

> 每次重启比较麻烦，可以设置nginx开机自启，https://www.cnblogs.com/piscesLoveCc/p/5867900.html

访问url： ip/images/123.jpg，正常访问

3. 关于Nginx的配置文件访问路径问题。
当使用vsftpd上传到相应目录文件后，怎么取访问呢？
需要我们在nginx的安装目录中找到nginx.conf文件，我的配置文件所在目录：/usr/local/nginx/conf/nginx.conf,打开该文件修改：
```
 #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
        #   root   html;之前的配置
        #   现在的配置在下面：访问路径：/home/ftpuser/wwww；
            root   /home/ftpuser/www;
            index  index.html index.htm;
        }
```
修改完成后，重新加载ngnix：
```
[root@localhost sbin]# ./nginx -s reload

```
4. 通过上面的一步，或许还会出现403 Forbidden的错误
原因分析：权限问题
解决方案：同样在`nginx.conf`的头部加入一行：
```
user root;
```
同样的，重新加载配置文件，启动nginx，此时我们就可以正常访问了。


### 二. vsftpd的配置问题

1. 需要注意的一个问题是，防火墙开启21端口，详细参考：[前一章节]()
2. 关于修改iptables和vsftpd.conf配置文件后，重启服务的命令，centos或者Fedora等高版本与其它版本有不通。
```
#  /bin/systemctl start vsftpd.service
```
否则会报：
```
Redirecting to /bin/systemctl restart  vsftpd.service
```

3. 关于使用FileZilla上传文件出现553 Could not create file错误
原因分析：可能是linux文件的访问权限问题。
解决办法：进去linux，找到访问文件的目录，找到相应的文件，鼠标右键点击属性，权限，设置权限为可写，重启一下vsftpd服务即可。然后重新使用FileZilla重新上传一下文件，即可。
