---
title: jQuery.form和jQuery.validate的使用
date: 2015-11-20
top: 3
categories: [前端]
tags: [jQuery]
description: "前端学习中的一些收获。"
---
<!--more-->
## jquery.form的两种提交方式
### 方式1：ajaxForm
####ajaxForm方式必须先绑定表单，它一般在$(document).ready(function(){})中定义，他能让表单在不刷新页面的情况下post到目标，例：

``` javascript
$(document).ready(function(){
   $('#updateForm').ajaxForm(function(){
        alert("ajaxForm提交完成");
   }); 
});
```

### 方式2：ajaxSubmit
####ajaxSubmit方式是以相应事件来通过ajax方式提交表单，比如单击某个按钮来触发该表单的提交，例：

``` javascript
$('#btnTest').on('click',function(){
    $('#updateForm').ajaxSubmit(function(){
        alert("ajaxForm提交完成！");
    });
});
```
## jquery.validate + jquery.form提交的三种方式
#### 方式1：通过jquery.validate的submintHandler选项，即当表单通过验证时执行回调函数，在这个回调函数中通过jquery.form来提交表单；


----------


#### 方式2：通过jquery.form的beforeSubmit，即在提交表单前执行回调函数，这个函数如果返回true，则提交表单，如果返回false，则终止提交表单，根据jquery.validate插件的valid()方法，可以通过jquery.form提交表单时来对表单进行验证；


----------


#### 方式3：通过jquery.validate的validate()方法，这个方法的好处是对表单验证的控制更加自由。






