---
title: Java之自动拆装箱及享元模式应用
date: 2016-12-29
top: 7
categories: [java]
tags: [autoboxing,提升]
description: "autoboxing是compiler suger（编译器蜜糖）之一，分析autoboxing，避免“蜜糖”给我们带来便利的同时，埋下陷阱。"
---
<!--more-->


[toc]

首先，来说一下关于编译器蜜糖（compiler suger）的问题，它给我们带来便利的同时，也埋下了一些陷阱，像foreach的增强，自动拆装箱等，本节
一起来学习一下蜜糖之一的自动拆装箱机制。

### 一. 静态导入

1. 静态导入
- import语句可以导入一个类或某个包中的所有类
- import static语句导入一个类中的某个静态方法或所有静态方法

2. 举例
```java
import static java.lang.Math.max;
import static java.lang.Math.*;
```

### 二. 可变参数

1. 特点
- 只能出现在参数泪飙的最后；
- ...位于变量类型和变量名之间，前后有无空格都可以；
- 调用可变参数的方法时，编译器为该可变参数隐含创建一个数组，在方法体中以数组的形式访问可变参数。

2. 举例

```java
public static int add(int x,int... args){
    int sum = x;
//        for (int i = 0; i < args.length; i++) {
//            sum = sum + args[i];
//        }
    //使用for循环增强
    for (int arg :
            args) {
        sum += arg;
    }
    return sum;
}


```

2. overload和override区别
http://developer.51cto.com/art/201106/266705.htm
http://www.cnblogs.com/whgw/archive/2011/10/01/2197083.html
重写Overriding是父类与子类之间多态性的一种表现，重载Overloading是一个类中多态性的一种表现。很重要的一点就是，Overloaded的方法是可以改变返回值的类型。


### 三. 自动拆装箱及享元模式及其应用

1. 前言
介绍自动拆装箱前，要先理解基本数据类型（Primitive）和对象（Object）的相关概念，在java中，所要处理的东西几乎都是对象，但是基本数据类型不是对象，向我们使用的`int`、`double`、`boolean`、`byte`、`long`等等，而他们都对应了一个引用的类型，称为装箱基本类型，也称为包装类。

![autoboxing](https://raw.githubusercontent.com/williamHappy/FileRepo/master/hexo/20161225/Java004/img/autoboxing.png)
2. 自动拆装箱
<p style="color:blue;">装箱：根据数据创建对应的包装对象</p>

```java
Integer i = new Integer(3);
Integer j = 4;//jdk1.5之后可以通过这种方式自动装箱
```
<p style="color:blue;">装箱：根据数据创建对应的包装对象</p>

```java
int  index2 = j.intValue();
int  index1 = i;//自动拆箱
```

而自动拆装箱的功能事实上是编译器帮的忙，编译器在编译时期依您所编写的语法，来决定是否进行拆装箱操作。

JDK1.5 为Integer 增加了一个全新的方法：public static Integer valueOf(int i) 在自动装箱过程时，编译器调用的是static Integer valueOf(int i)这个方法 于是:
`Integer a=3; ==> Integer a=Integer.valueOf(3);`

> 此方法与new Integer(i)的不同处在于: 
方法一调用类方法返回一个表示 指定的 int 值的 Integer 实例。方法二产生一个新的Integer 对象。

3. 缓冲机制的原理
关于自动拆装箱的缓冲机制原理的详细解释，看下面的blog，博主很解释的很给力勒！
http://blog.csdn.net/u010293702/article/details/44621675
还有其他的一些包装类详情，也可以关注这篇blog，下面举例举实例加深理解

4. 举例
```java
package com.william.test;

/**
 * Created by william on 2016/12/25.
 */
public class AutoBoxTest {

    public static void main(String[] args){
        Integer integer = 3;//数据装箱
        //实际的操作为：Integer iObj = new Integer(3);
        System.out.println(integer+12);//自动拆箱做加法

        String str1 = new String("abc");
        String str2 = new String("abc");
        String str4 = "abc";
        String str5 = "abc";

        Integer i1 = 120;
        Integer i2 = 120;
        Integer i3 = 130;
        Integer i4 = 130;

        System.out.println("str1==str2:"+(str1==str2));
        System.out.println("str4==str5:"+(str4==str5));//串常量所生成的变量，其中所存放的内存地址是相等的
        System.out.println("str1.equals(str2):"+str1.equals(str2));//String中重写equals（）方法，比较其内容
        System.out.println("str1.equals(str4):"+str1.equals(str4));


        System.out.println("i1==i2:"+(i1==i2));
        System.out.println("i3==i4:"+(i3==i4));//至于i3==i4返回false的原因就是因为Integer的缓冲机制导致的
        System.out.println("i3.equals(i4):"+i3.equals(i4));//Integer中重写equals（）方法，使其比较其值，而不是用来比较两个引用变量是否指向同一个对象



    }

}


output:
15
str1==str2:false
str4==str5:true
str1.equals(str2):true
str1.equals(str4):true
i1==i2:true
i3==i4:false
i3.equals(i4):true

```

对于上面用到的`equals()`,`==`另外还有像`hashcode`,`instanceof`,`compareTo`等比较方法的区别，感兴趣的可以自己查阅学习，这里只说一下`equals()`和`==`的区别。

- “==”比较两个变量本身的值，即两个对象在内存中的首地址。
- equals()在Object类中同样是用来比较‘地址’的，但是在String，Integer等包装类中重写了equals()方法，使其比较的是内容，这也就是上面的实例中，使用equals比较返回true的原因。

对于其详细解释：
http://blog.csdn.net/t12x3456/article/details/7341515


5. 享元模式与Integer类
举个简单的例子：
*天天跟MM发短信，手指都累死了，最近买了个新手机，可以把一些常用的句子存在手机里，要用的时候，直接拿出来，在前面加上 MM的名字就可以发送了，再不用一个字一个字敲了。共享的句子就是Flyweight，MM的名字就是提取出来的外部特征，根据上下文情况使用。*

> 这里说到的享元模式和Integer类，在上面的例子中，我们学习到了Integer会把常用的-128~127之间的数字在装箱后会被缓存起来，当下次对同样的数字装箱时，两个Interger对象是相等的，因为他们指向的是同一块内存，即是使用同一个对象，而不再这个范围中的数字，在自动装箱时，是不会被缓存的，即重新创建一个对象，所以当数字大于127时，他们用‘==’比较，返回的是false，即他们在内存中的首地址是不一样的。

通过以上所属，享元模式，是对于哪些非常小但又需要在系统的很多地方都需要到他的时候，我们可以把它缓存起来，以便在诗词再次使用，减少了创建对象的开销。

备注：感谢引用的以上各位博主的博客，让我学习到了很多。。。