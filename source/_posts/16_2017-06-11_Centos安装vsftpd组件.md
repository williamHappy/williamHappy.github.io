---
title: Centos安装vsftpd组件
date: 2017-06-11
categories: [后端]
tags: [Linux,vsftpd]
description: "Linux还是要好好学的，要不安装个东西太累了！"
cover_picture: http://imgsrc.baidu.com/imgad/pic/item/7aec54e736d12f2e9525d04744c2d562853568f0.jpg
---
<!--more-->

转载至vsftpd安装手册。。。

[toc]

### 一. 安装vsftpd组件
安装完后，有/etc/vsftpd/vsftpd.conf 文件，是vsftp的配置文件。
```
[root@bogon ~]# yum -y install vsftpd
```

### 二. 添加一个ftp用户
此用户就是用来登录ftp服务器用的。
```
[root@bogon ~]# useradd ftpuser
```

这样一个用户建完，可以用这个登录，记得用普通登录不要用匿名了。登录后默认的路径为 /home/ftpuser.	

### 三. 给ftp用户添加密码。
```
[root@bogon ~]# passwd ftpuser
```
输入两次密码后修改密码。

### 四. 防火墙开启21端口

因为ftp默认的端口为21，而centos默认是没有开启的，所以要修改iptables文件
```
[root@bogon ~]# vim /etc/sysconfig/iptables
```

在行上面有22 -j ACCEPT 下面另起一行输入跟那行差不多的，只是把22换成21，然后：wq保存。
```
-A INPUT -p tcp -m multiport --dport 20,21  -m state --state NEW -j ACCEPT  --开启20,21端口
-A INPUT -p tcp -m state --state NEW -m tcp --dport 21 -j ACCEPT            --开启21主动端口
-A INPUT -p tcp --dport 30000:31000 -j ACCEPT            --开启被动端口
```


还要运行下,重启iptables

```
[root@bogon ~]# service iptables restart
```

### 五. 修改selinux
外网是可以访问上去了，可是发现没法返回目录（使用ftp的主动模式，被动模式还是无法访问），也上传不了，因为selinux作怪了。
修改selinux：
执行以下命令查看状态：
```
[root@bogon ~]# getsebool -a | grep ftp  
allow_ftpd_anon_write --> off
allow_ftpd_full_access --> off
allow_ftpd_use_cifs --> off
allow_ftpd_use_nfs --> off
ftp_home_dir --> off
ftpd_connect_db --> off
ftpd_use_passive_mode --> off
httpd_enable_ftp_server --> off
tftp_anon_write --> off
[root@bogon ~]# 
```

执行上面命令，再返回的结果看到两行都是off，代表，没有开启外网的访问
```
[root@bogon ~]# setsebool -P allow_ftpd_full_access on
[root@bogon ~]# setsebool -P ftp_home_dir on
```
 

### 六. 关闭匿名访问
修改/etc/vsftpd/vsftpd.conf文件：
```
anonymous_enable=NO
```

 
重启ftp服务：
```
[root@bogon ~]# service vsftpd restart
```

### 七. 开启被动模式
默认是开启的，但是要指定一个端口范围，打开vsftpd.conf文件，在后面加上
> pasv_min_port=30000
pasv_max_port=30999


表示端口范围为30000~30999，这个可以随意改。改完重启一下vsftpd
由于指定这段端口范围，iptables也要相应的开启这个范围，所以像上面那样打开iptables文件。
也是在21上下面另起一行，更那行差不多，只是把21 改为30000:30999,然后:wq保存，重启下iptables。这样就搞定了。

### 八. 设置开机启动vsftpd ftp服务

```
[root@bogon ~]# chkconfig vsftpd on
```

> 相关问题和踩过的坑，查看[下一章节](http://willhappy.cn/2017/06/11/%E5%AE%89%E8%A3%85Nginx%E5%92%8Cvsftpd%E7%9A%84%E5%9D%91/)。


