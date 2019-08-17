---
title: java重点易错知识总结笔记
date: 2019-04-19
categories: [通用]
tags: [interview]
description: "易错点，重点笔记"
cover_picture: http://oss.willhappy.cn/hexo/cover_pic/cover_picture_34.jpg

---

  平常经常用的知识点，但是，还是易犯的错误，像高中一样，把他记录下来，不时地温习，相信我们可以彻底掌握他们。

<!--more-->

[toc]

### java基础部分

1. `==` 与 `equals`
  `==`: 它是用来判断两个值是否相等的。基本类型判断的是值是否相等，引用类型判断的是内存地址的值是否相等。
  `equals`：分equals方法是否被覆盖两种情况判断
  
    - 没被覆盖，则与`==`作用相同，比较两个对象的内存地址是否相同.

    - 覆盖了，则是根据覆盖的方法，判断两个对象的内容是否相同，相同即返回false.

2. 为什么重写equals时必须重写hashCode方法？
  首先，我们要确定几个规定：

    - 如果两个对象相等，则hashcode一定也是相同的

    - 两个对象相等，equals方法返回true

    - 有相同的hashcode, 但是不一定相等

  然后，我们再根据源码分析下：

```java
public native int hashCode();

public boolean equals(Object obj) {
    return (this == obj);
}
```

  `hashCode()`方法是一个本地native方法，返回的是对象引用中存储的对象的内存地址，而`equals()`是利用==来比较的也是对象的内存地址。从上边我们可以看出，`hashCode()`和`equals()`是一致的。即如果`equals()`方法返回true, 那么`hashCode()`的返回值也必须是一样的。
  通过以上的结论，假如有两个对象实例，我们其类重写了`equals()`方法，使其满足属性值相同，返回true，那么，如果不重写`hashCode()`方法，返回的依然是两个对象的内存地址，显然是不相等的。这就出现了`equals()`方法相等，但是`hashCode()`返回值不相等的情况，不符合hashCode的规则。当然，在集合框架中，这会引起严重的问题。

  当你把对象加入 HashSet 时，HashSet 会先计算对象的 hashcode 值来判断对象加入的位置，同时也会与其他已经加入的对象的 hashcode 值作比较，如果没有相符的hashcode，HashSet会假设对象没有重复出现。但是如果发现有相同 hashcode 值的对象，这时会调用 equals（）方法来检查 hashcode 相等的对象是否真的相同。如果两者相同，HashSet 就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。（摘自我的Java启蒙书《Head first java》第二版）这种情况下，我们单独重写类的equals()使其返回相等，但是，hashCode返回未必相等，就导致会散列到不同的数组上，进而导致HashSet可能存在重复的值。
3. 为什么Java中只有值传递？
按值调用(call by value)表示方法接收的是调用者提供的值，而按引用调用（call by reference)表示方法接收的是调用者提供的变量地址。一个方法可以修改传递引用所对应的变量值，而不能修改传递值调用所对应的变量值。
Java程序设计语言总是采用按值调用。也就是说，方法得到的是所有参数值的一个拷贝，也就是说，方法不能修改传递给它的任何参数变量的内容。
几个例子，参考[为什么 Java 中只有值传递？][1]

[1]: https://github.com/Snailclimb/JavaGuide/blob/master/docs/essential-content-for-interview/MostCommonJavaInterviewQuestions/%E7%AC%AC%E4%B8%80%E5%91%A8%EF%BC%882018-8-7%EF%BC%89.md
