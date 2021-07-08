---
title: 【转】2021文章分享
date: 2021-01-26
categories: [分享]
tags: [articles]
description: 工作中遇到的特殊业务场景处理以及一些重点知识场景应用，还有一些比较优秀的某个知识点分享等
cover_picture: http://oss.willhappy.cn/hexo/cover_pic/cover_picture_32.jpg

---

自己查资料阅读到写的比较好的文章分享出来大家一起看。也欢迎大家支持原创！同时也方便自己以后查阅翻看。其内容包含工作中遇到的特殊业务场景处理以及一些重点知识场景应用等，望共勉。

<!--more-->

[toc]

#### 前言

- 分享页分为几大类：`项目构建`,`框架`,`前端`,`后端`等
- 可在本页面使用`Ctrl+F`按日期搜索，也可以按主题搜索，`ex:` 2018/07/01, java
- 每大类按日期降序排列

#### 项目构建

#### 架构[1]

**2021/01/26-DDD** 
> 场景：技术分享使用
- [为什么DDD是设计微服务的最佳实践][1-1]
- [DDD 分层架构最佳实践][1-2]
- [基于 DDD 的微服务设计和开发实战][1-3]
- [领域驱动设计(DDD)实践之路(四)：领域驱动在微服务设计中的应用][1-4]
- [限界上下文：定义领域边界的利器][1-5]
- [leave-sample(本代码源于极客时间《DDD实战课》)][1-6]

#### 框架[2]
**2021/07/08-mybatis大数据查询优化**
> 场景：大数据量查询导出文件
- [MyBatis（八）：懒加载][2-9]
- [MyBatis 延迟加载（懒加载）一篇入门][2-10]

**2021/07/08-easyExcel使用**
> 场景：大数据量复杂表格导出
- [POI如何高效导出百万级Excel数据？][2-4]
- [大数据量excel解析工具性能对比][2-5]
- [大数据量写入Excel，试试EasyExcel][2-6]
- [java中多种写文件方式的效率对比实验][2-7]
- [异步 excel 导出组件设计和实现][2-8]

**2021/05/18-springboot异步异常处理**
> 场景：异步出现异常
-[SpringBoot学习笔记（十七：异步调用）][2-3]


**2021/04/23-@Vaild和@Validatd**
> 场景：公司做批量导入txt文件自定义校验
- [如何在SpringBoot中做参数校验][2-1]
- [SpringBoot利用@Validated和@Valid进行校验参数][2-2]

#### 前端[3]

#### 后端[4]
**2021/06/07-海量数据插入处理**
> 场景：分库分表方案
- [分布式数据库中间件Sharding-JDBC][4-19]

**2021/07/08-集合效率问题**
> 场景：大数据量遍历
- [list与map遍历效率浅析][4-18]

**2021/06/07-反射**
> 场景：自定义模板
- [java反射调用get/set方法，你还在拼接方法名吗？][4-15]
- [如何利用缓存机制实现JAVA类反射性能提升30倍][4-16]

**2021/06/07-多线程并发编程**
> 场景：异步处理场景
- [线程池多任务的执行顺序][4-12]
- [线程池其实看懂了也很简单][4-13]
- [三分钟弄懂线程池执行过程][4-14]
- [Java 四种线程池][4-17]

**2021/06/07-海量数据插入处理**
> 场景：分库分表方案
- [MySQL分库分表方案][4-7]
- [不用找了，大厂在用的分库分表方案，都在这里！][4-8]
- [MySQL分库分表，写得太好了！][4-9]
- [干货丨数据库分库分表基础和实践][4-10]
- [分库分表的几种常见形式以及可能遇到的难][4-11]

**2021/05/18-多线程并发编程**
> 场景：批量导入异步处理
- [java8线程池][4-6]

**2021/04/23-分批次插入java处理**
> 场景：分批次插入数据
- [list等分效率对比][4-4]
- [分页算法][4-5]

**2021/04/23-海量数据插入处理**
> 场景：公司做批量导入txt文件数据时使用
- [mysql批量插入数据，一次插入多少行数据效率最高？][4-1]
- [Mybatis批量插入数据的三种方式效率对比][4-2]
- [Mysql千万级别数据批量插入只需简单三步！][4-3]

#### 测试[5]
> 场景：做单元测试，要求覆盖率统计，如何正确编写单元测试
- [mock测试及jacoco覆盖率][5-1]

#### API[6]

#### 工具[7]
**2021/07/08-JVM性能诊断工具**
> 场景: 提交历史错乱，rebase为什么危险
- [为什么你应该停止使用 Git rebase 命令][7-4]

**2021/07/08-JVM性能诊断工具**
> 场景：监控导出大数据内存占用情况
- [性能诊断利器 JProfiler 快速入门和最佳实践][7-1]
- [windows下查找java应用程序CPU与内存过高][7-2]
- [JDK工具（查看JVM参数、内存使用情况及分析等）][7-3]






[1-1]: https://www.jianshu.com/p/e1b32a5ee91c
[1-2]: https://xie.infoq.cn/article/f88324b09cd9db4214ac153cf
[1-3]: https://www.infoq.cn/article/s_lfulu6zqodd030rbh9
[1-4]: https://segmentfault.com/a/1190000038480392
[1-5]: https://time.geekbang.org/column/article/149950
[1-6]: https://github.com/ouchuangxin/leave-sample

[2-1]: https://blog.csdn.net/qq_39609993/article/details/114288378
[2-2]: https://blog.csdn.net/u014082714/article/details/107160281
[2-3]: https://juejin.cn/post/6850418116464230408
[2-4]: https://zhuanlan.zhihu.com/p/60347814
[2-5]: https://www.cnblogs.com/cnsec/p/13286591.html
[2-6]: https://www.sundayfine.com/easyexcel/
[2-7]: https://developer.aliyun.com/article/553054
[2-8]: https://xie.infoq.cn/article/3a73ce87b824861365862879d
[2-9]: https://zhuanlan.zhihu.com/p/37259948
[2-10]: https://juejin.cn/post/6844904062362583054


[4-1]: https://www.geek-share.com/detail/2777135663.html
[4-2]: https://blog.csdn.net/WillLiaowh/article/details/106028882?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-4&spm=1001.2101.3001.4242
[4-3]: https://zhuanlan.zhihu.com/p/75432448
[4-4]: https://www.imooc.com/article/41647
[4-5]: https://blog.csdn.net/c5113620/article/details/83106888
[4-6]: https://blog.csdn.net/wy11933/article/details/80399562
[4-7]: https://zhuanlan.zhihu.com/p/84224499
[4-8]: https://www.huaweicloud.com/articles/8ac1dc6dfe005bdaf76f7d1d2136276a.html
[4-9]: https://database.51cto.com/art/201809/583857.htm
[4-10]: https://www.infoq.cn/article/zmlcbpihothwjeqmzd4i
[4-11]: https://www.infoq.cn/article/key-steps-and-likely-problems-of-split-table
[4-12]: https://blog.csdn.net/Mr_25kjiang/article/details/105815054
[4-13]: https://www.cnblogs.com/jajian/p/11442929.html
[4-14]: https://juejin.cn/post/6866054685044768782
[4-15]: https://www.cnblogs.com/1ning/p/10936129.html
[4-16]: https://segmentfault.com/a/1190000020986852
[4-17]: https://www.cnblogs.com/zhujiabin/p/5404771.html
[4-18]: https://juejin.cn/post/6844903937246494728
[4-19]: https://blog.gzj.me/2017/11/01/%E5%88%86%E5%B8%83%E5%BC%8F%E6%95%B0%E6%8D%AE%E5%BA%93%E4%B8%AD%E9%97%B4%E4%BB%B6Sharding-JDBC/

[5-1]: https://www.cnblogs.com/yjmyzz/p/mockito-and-jacoco-tutorial.html


[7-1]: https://segmentfault.com/a/1190000017795841
[7-2]: https://blog.csdn.net/gavin_john/article/details/52458542
[7-3]: https://www.cnblogs.com/z-sm/p/6745375.html
[7-4]: https://zhuanlan.zhihu.com/p/29682134