---
title: 主页部署docker+ng+tomcat+https
date: 2018-06-13
categories: [运维]
tags: [Linux,docker,nginx,https]
description: "将博客主页搬到docker上"
cover_picture: http://oss.willhappy.cn/hexo/cover_pic/cover_picture_30.jpg

---

上一章[docker初识][1]结合docker的文档，将博客主页所需的生产环境镜像及容器准备好了，这一章主要是写一些在博客部署到线上时出现的一些问题，从而对使用容器部署有一个更深层次的理解。

<!--more-->

[toc]

### 前言
之前想着是使用一个纯净的linux镜像，然后使用dockerfile构建一个自己博客环境镜像的，但是自己的水平有限，目前就使用拉去官方镜像，进行一些配置，已达到自己的生产环境需求。后面有机会学习的话，会构建一个自己的镜像出来，期待ing。

另外，我是使用的服务器是google平台，搭建的服务器环境是基于debian(stretch 9.4)，所以很多相关的服务器操作和centos有所不同，比如包管理工具apt-get,vim([需要安装][2],[iptables][3])

### 一. 博客部署到tomcat容器

这个还是比较简单的，前面[一章][1], 我们已经将主机中目录下的/usr/lcoal/docker/tomcat/apps挂载到容器的/apps, 那么我们可以直接将war包放到/usr/local/docker/tomcat/apps目录下，即可进入容器内部，然后进行相关配置。

```bash
$ cd /usr/local/docker/tomcat/apps
# 上传博客war包
# 进入tomcat 容器
$ docker exec -it tomcat_will /bin/bash

# tomcat容器shell操作，进入容器后，直接是在/usr/local/tomcat/目录下
# 进入webapps/apps/目录下，会发现我们刚刚上传到宿主机的war包，把他移动到webapps目录下，会自动解压缩
# 进入/usr/local/tomcat/conf/目录下，配置server.xml文件
```

配置`server.xml`文件：

```xml
<Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true"
            xmlValidation="false" xmlNamespaceAware="false">
    <Context path="" docBase="/usr/local/tomcat/webapps/XX你的项目名" reloadable="true" />
</Host>
```

推出tomcat容器，回到宿主机

```bash
# 重启tomcat容器
$ docker resatrt tomcat_will
```

测试访问：http://35.197.142.179:8080/ ，如果访问到你的项目说明部署成功。
如果出现无法访问的情况，请查看是否开放服务器端口号。可使用站长工具的[端口扫描工具][15]来查看是否真正开放。
google平台的话，可在vpc网路中[创建防火墙规则][4]来开放端口号。

### 二. nginx+https相关配置

如果单纯只是http配置的话，相对简单，可参考[文章][5], 这里我们重点介绍结合配置https.

#### 说明
docker的nginx容器，配置文件在`/etc/nginx`目录下，有`nginx.conf`主配置文件，会加载`conf.d/*.conf`目录的所有配置文件，通过`nginx.conf`可以看出。

```conf
http {
  ...
  include /etc/nginx/conf.d/*.conf;
  ...
}
```
这也就是为什么我们在运行容器的时候，挂载`server.conf`到`conf.d/`目录下，这样可以通过主配置文件将其加载进来，我们就可直接在宿主机上目录下更改`server.conf`.当然，这仅限于更改http块里面的内容。

另外参考我的[另一篇文章][6]来配置https,因为知道在哪里配置，配置的内容自然就差不多了。

#### 上传证书
通过我们挂在目录，我们将证书上传docker容器，挂载文件主要是方便容器和宿主机的文件共享，避免了频繁的在宿主机和容器间进行文件的拷贝。

```bash
# 进入daocker nginx容器
$ docker exec -it nginx_will_v2 /bin/bash
# 为了方便，我们把证书文件统一放到/etc/nginx/cert目录下
```

#### 修改配置
配置挂载文件/etc/nginx/conf.d/server.conf，当然可在容器内修改，也可在宿主机上修改，文件是共享的（我的理解，没有完全实践）。
server.conf添加内容如下：

```conf
# 和我们前面文章中的https配置基本一致，需要更改的就是upstream中的server,更改为你的ip地址，因为容器的隔离技术，再使用localhost可能转发失败（可自己尝试）。
server {
    listen 80;
    listen 443;
    server_name willhappy.cn;   #拦截的域名
    ssl on;
    root html;
    index index.html index.htm;
    ssl_certificate   cert/XX.pem;  #你自己申请的证书文件
    ssl_certificate_key  cert/XX.key;  #私钥文件
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

    location / {
        proxy_pass http://whome;    #提供数据服务的服务器
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

upstream whome{
    server ip地址:8080;
}
```

#### 访问测试
通过 https://willhappy.cn/ ，访问到自己的项目即表示配置成功。

#### 自己挖的坑
通过nginx容器名称（nginx_will_v2）可以看到，这是我运行的第二个容器，第一个容器，我在创建时只开放了80端口，所以当一切都配置好之后，发现通过443端口（也就是通过https方式）访问，怎么都无法访问。第一时间想到的时宿主机没有开放443端口，没想到容器的443端口没有映射到443端口上，通过在google云平台配置防火墙，开放443端口，但是，依然无效，这时就陷入了一个巨大的坑，难道云平台上配置无效？那就去服务器上手动开放443端口。鉴于服务器是debian版本，iptables配置有所不同，结果去单独配置443端口时，把其他的端口全都抹掉了，其中包括docker和宿主机的防火墙设置都清除掉，导致容器都无法启动。这中间经历艰辛，无从言起，记录下，避免下次再出现。

1. **不明原理，贸然根据网上教程更改服务器（debian）的iptables配置。**
  更改参考[文章1][7],[文章二][8], 根据这两篇文章修改，虽然没有最终完成修改，但是依然更改了端口号的配置。发现情况不妙；
  发现情况不妙后，准备恢复防火墙设置，参考[iptables配置][9], 因为不知道哪个是原来防火墙的最终配置，所以选择了`/etc/iptables.up.rules`进行恢复。
  从文件恢复IPtables规则：

```
$ sudo iptables-restore < iptables.up.rules
```

这样虽然挽回了部分端口规则，但是docker的防火墙规则被破坏，出现了问题2.

2. **docker 启动容器报"No chain/target/match by that name"错误，无法启动容器。**
  同样参考了几篇文章：[文章][10]，但是都是基于centos修改防火墙规则的，但是已经不敢再去动debian的规则了，怕再出问题。所以继续找。
  google搜索最终看到一种[解决方法][11],即重启docker服务，即可自动恢复docker关于防火墙的配置，测试，最终解决，QAQ。
  
```
# 重启docker服务，须在所有容器停止后重启
$ sudo systemctl restart docker
```

3. **至此，debian防火墙规则恢复到原来的状态，冷静下来后，恍然发现，是因为nginx容器的443端口没有映射到宿主机上，再次泪奔，问题忽然这么明朗、容易，或许，总是要经历些什么，才能成长吧！**

```bash
# 重新创建运行开放80和443端口的容器，这才有了nginx_will_v2这个容器
$ docker run -p 80:80 -p 443:443 --name nginx_will_v2 -v $PWD/conf/server.conf:/etc/nginx/conf.d/server.conf -v $PWD/www:/www -v
```

访问 https://willhappy.cn/ 测试成功。

解决过程艰辛，关于debian系统的各种骚操作，还得要深入学习，传送[debian官网][12], 不然，随便一操作，那都是一个坑啊，泪奔。

#### more
关于nginx, 可参考[nginx中文文档][13],查看相关参数说明。
通过nginx配置更多负载均衡，动静分离，参考[文章][14]

### 后记

相关命令

```
$ lsb_release -a          #查看debian版本信息
$ sudo systemctl restart docker     #重启docker服务（debian）须在所有容器停止后重启
```

相关工具
- [站长工具][15]: 本章所用功能->端口扫描
- [debian官网][12]: 关于iptables部分




[1]: http://blog.willhappy.cn/2018/06/11/29_2018-06-11_docker%E5%88%9D%E8%AF%86/
[2]: https://blog.csdn.net/weixin_39800144/article/details/79231002
[3]: https://wiki.debian.org/iptables
[4]: https://console.cloud.google.com/networking/firewalls/list?project=beaming-oarlock-197105&tab=INGRESS
[5]: https://blog.csdn.net/mlc1218559742/article/details/53117520
[6]: http://blog.willhappy.cn/2018/04/25/21_2018-04-25_%E5%B0%8F%E7%BB%BF%E9%94%81https%E7%9A%84web%E5%AE%B9%E5%99%A8%E9%85%8D%E7%BD%AE/
[7]: https://www.centos.bz/2017/10/debian%E9%85%8D%E7%BD%AEiptables/
[8]: https://my.oschina.net/winHerson/blog/143465
[9]: http://www.codebelief.com/article/2017/08/linux-25-useful-iptables-firewall-rules/
[10]: http://ystyle.top/2015/09/24/centos-7-docker-qi-dong-bao/
[11]: https://support.plesk.com/hc/en-us/articles/115000186754-Docker-container-does-not-start-with-error-No-chain-target-match-by-that-name
[12]: https://www.debian.org/
[13]: http://www.nginx.cn/doc/index.html
[14]: http://www.runoob.com/w3cnote/linux-nginx-tomcat.html
[15]: http://tool.chinaz.com/
