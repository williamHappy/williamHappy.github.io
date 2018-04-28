---
title: GitHub+Hexo搭建个人博客
date: 2015-07-24
categories: [通用]
tags: Github/hexo
description: 使用Github空间搭建Hexo博客，写博客，分享交流。
cover_picture: http://img.willhappy.cn/hexo/cover_pic/cover_picture_2.jpg
---

<!--more-->

[TOC]

### 1.hexo介绍
看到同学使用github+hexo搭建了属于自己的博客，眼馋，弄了好久了，感觉有点眼高手低了，一直没来得及写一下自己搭建的流程，心血来潮，下边来简单介绍下搭建流程，也方便自己以后review，也可以和大家共享交流技术问题。
可以关注我的blog,  
>地址：http://blog.willhappy.cn
Hexo简介：
>Hexo是一个基于node.js快速，简介且高效的博客框架，可以将Markdown文件快速的生成静态网页，托管到github pages上。--《摘自Judas的hexo博客》

### 2.搭建前准备
所需软件
> - git: http://git-scm.com/
> - node.js：http://nodejs.org/

- 运行 Git-2.7.0.2-64-bit.exe，安装完成后，验证git安装是否成功：
```
git --version
```
若出现git的版本号级表示安装成功，即
> git version 2.7.0.windows.2

- 运行 node-v5.7.0-x64.msi，安装完成后，验证node.js是否安装成功，在git bash中执行：
```
npm -v
```
若出现版本号，即代表成功，如：
> 1.4.3

### 3.安装hexo
- 可以先创建一个文件夹，命名随自己，这里假设命名为hexo，
在hexo目录下，打开git bash，执行：
```
npm install -g hexo  //安装hexo
```
- 安装成功，继续执行命令进行hexo初始化：
```
hexo init            //hexo初始化
```
网速决定你的等待时间，可能需要几分钟，成功后会在hexo目录下生成相关目录。
 - 现在，我们就可以启动hexo本地服务，命令：
 ```
 hexo s
 ```
 注意（踩过的坑）：
 > ERROR Local hexo not found in E:\GitHub\hexo
 
 出现这个错误的原因可能是：仔细观察是hexo的生成目录中缺少了node_modules文件夹，所以这个文件夹没有更新上去，可以继续在git bash中执行:
 ```
 npm install
 ```
 重新安装以后，继续执行：hexo s  即可。
 
 现在可以使用浏览器访问：http://localhost:4000/。
 -在git bash中按Ctrl+C即可停止hexo服务。
 现在，我们便完成了静态博客的搭建。
 
 ### 4.书写blog文章
 可以在git bash中使用命令来新建文件

    hexo new "newBlog"    //新建文章
    
然后来写文章内容，但是这样过于麻烦，因为hexo 博客是支持markdown语法格式的，我们可以使用相关的编辑器编写好md文档后，将其放在hexo目录下的\source\\_posts文件夹下，此文件夹下已经有了一个hello-world.md文件。然后我们执行：
```
hexo g          //生成静态页面
```
重新生成一下静态页面，然后执行hexo s启动服务。
通过http://localhost:4000/，  可以访问我们书写了新文章的博客了。

### 5.配置到GitHub
到这里，我们只能通过自己启动本地服务去访问博客，别人访问不到我们的blog网站，下面介绍如何将hexo部署到github pages上，可实现任何人都可以访问自己的blog文章。

- 首先，在github上创建一个特殊的仓库，仓库名称为：

> **用户名**.github.io

然后在hexo目录下找到_config.yml文件，打开在最后修改如下代码：
```
deploy:
  type: git
  repo: https://github.com/用户名/用户名.github.io.git
  branch: master
```
 
 最后，在git bash中执行命令，提交到github：

    hexo d

如果不出意外的话，在浏览器中可以通过http://用户名.github.io/  来访问你的博客了。

hexo博客整体命令操作流程图：
![hexoBlogProces](https://raw.githubusercontent.com/williamHappy/FileRepo/master/hexo/20161124/HexoBlog/img/hexoBlogProces.gif)

### 6.主题修改
- 在这里 https://github.com/hexojs/hexo/wiki/Themes  挑选好我们心动的主题。
- 在hexo目录下，使用git bash 执行命令：
```
git clone 主题地址
```
- 下载好主题后，同样在hexo目录下打开配置文件:_config.yml，把对应的主题目录名改下：

> theme: 主题名称

- 更改好主题目录名后

我们执行hexo g重新生成下静态页面，然后hexo s启动本地服务，重新访问：http://localhost:4000/，查看新主题的效果。

- 确认无误后，我们执行
```
hexo d
```
上传到github，最后通过 用户名.github.io访问查看最终效果。

### 7.hexo 相关命令

    hexo new page"pageName"     新建页面
    cls                         清屏
    hexo clean                  清理项目
    hexo g(generate)            生成静态页面至public目录
    hexo s(server)              开启预览访问端口
    hexo d(deploy)              将.deploy目录部署到GitHub
    hexo help                   查看帮助
    hexo version                查看Hexo的版本
    
    
### 8.绑定域名
可以访问：http://blog.willhappy.cn

### 9.更多
博客网站的主题优化，插件应用，博客文章书写技巧能相关内容敬请期待。。。
也可以关注hexo官方网站：hexo.io
