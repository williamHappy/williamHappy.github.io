---
title: jvm相关知识点
date: 2019-08-16
categories: [通用]
tags: [interview,jvm]
description: "易错点，重点笔记"
cover_picture: http://oss.willhappy.cn/hexo/cover_pic/cover_picture_34.jpg

---

  系统记录jvm相关知识点，形成系统性知识结构。

<!--more-->

[toc]

## jvm是什么

`Java虚拟机`（英语：`Java Virtual Machine`，缩写为JVM），一种能够运行Java bytecode的虚拟机，以堆栈结构机器来进行实做。
Java虚拟机有自己完善的硬体架构，如处理器、堆栈、寄存器等，还具有相应的指令系统。JVM屏蔽了与具体操作系统平台相关的信息，使得Java程序只需生成在Java虚拟机上运行的目标代码（字节码），就可以在多种平台上不加修改地运行。Java 虚拟机在执行字节码时,实际上最终还是把字节码解释成具体平台上的机器指令执行。

> **我的理解：** jvm其实就是一个软件，运行于操作系统之上
，当我们输入信息（字节码文件）时，它就帮我们解释成当前操作系统能理解的机器指令，由机器执行完，输出结果。这就是java程序执行的过程。

## jvm的

java虚拟机主要有三部分组成：
