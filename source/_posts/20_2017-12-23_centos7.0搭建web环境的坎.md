---
title: centos7.0搭建web环境的坎
date: 2017-12-23
categories: [后端, linux]
tags: [Linux]
description: "为了好好活着，还是得好好学习，沉淀啊！"

---
<!--more-->

[toc]

为了接下来能活的更好，再次埋头扎下了技术坑，一步一步来了，先从搭建生产环境走起，centos7.0 + tomcat8.5 + jdk1.8 + mysql5.7吧，简单的走流程的东西就不写了，只写一下在安装过程中走的坑。

### 一. 阿里云ECS服务器
### 二. mysql安装
1. 官网下载，安装到/usr/local/mysql目录下
官方地址：`https://dev.mysql.com/downloads/mysql/`
选择要下载的版本，上传到服务器即可，然后进行一系列的安装操作，下面具体说遇到的问题。
2. 启动服务服务时出错
```
service mysqld start
```
错误及解决方案：
![mysql_error](https://raw.githubusercontent.com/williamHappy/FileRepo/master/hexo/20171224/centos_env/img/mysql_error.png)

应该是缺少库，所以yum安装一下即可。

### 三. java环境安装配置
1. 环境变量的配置
安装过程不多说，环境变量配置如下：
```
[root@will ~]# vim /etc/profile
# 在export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL下添加
export JAVA_HOME=/usr/local/java
export JRE_HOME=$JAVA_HOME/jre
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
#保存后，执行source /etc/profile使其生效
```

安装配置完成，验证是否配置成功。
```
[root@will ~]# java -version
java version "1.8.0_151"
Java(TM) SE Runtime Environment (build 1.8.0_151-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.151-b12, mixed mode)
```

成功，java环境配置完成。

### 四. tomcat安装配置
1. 端口号开放
使用`iptables`进行端口号的开放。详情如下：
```
# 安装
yum install iptables-services

#配置防火墙
vim /etc/sysconfig/iptables

# 在 22端口号线面添加 如下端口，然后保存并退出 （：wq）
-A INPUT -p tcp -m state --state NEW -m tcp --dport 8080 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT

#重启防火墙
systemctl restart iptables.service

#查看 开放端口
iptables -L -n
```
> 注：这里必须要在 22端口号下面添加，不能添加到最后.

2. 设置tomcat服务，开机自启
具体实现命令如下：
```
[root@will ~]# vim /usr/lib/systemd/system/tomcat.service
#添加如下内容
[unit]
Description=Tomcat
After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/usr/local/tomcat/tomcat.pid
ExecStart=/usr/local/tomcat/bin/startup.sh
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
#添加完成，:wq保存
```
然后，cd到`/usr/local/tomcat/bin/`目录下，添加`setenv.sh`文件，具体操作如下：
```
[root@will bin]# cd /usr/local/tomcat/bin/
[root@will bin]# vim setenv.sh
#setenv.sh文件添加内容如下
#add tomcat pid
CATALINA_PID="$CATALINA_BASE/tomcat.pid"
#add java opts
JAVA_OPTS="-server -XX:PermSize=256M -XX:MaxPermSize=1024m -Xms512M -Xmx1024M -XX:MaxNewSize=256m"
#添加完成，:wq保存完成
```
设置开机自启，重启tomcat服务：
```
[root@will bin]# systemctl enable tomcat
Created symlink from /etc/systemd/system/multi-user.target.wants/tomcat.service to /usr/lib/systemd/system/tomcat.service.
[root@will bin]# systemctl restart tomcat
```

> 注：自己在设置tomcat开机启动时，还是遇到挺多问题的，不过写blog时，忘记了，以后有机会遇见还会再补充上的！

 `systemctl`相关命令，操作服务相关指令如下：
```
# 启动服务，stop停止
systemctl start tomcat
# 查看服务状态
systemctl status name
# 设置开机自启，disable删除
systemctl enable tomcat
# 重启服务
systemctl restart tomcat
```

### 五. 测试访问
至此，centos7web环境搭建完成，测试访问。
浏览器访问：
> http://ip:8080/

出现tomcat欢迎页面即搭建成功。
如有其他问题，也可备注添加^_^.

