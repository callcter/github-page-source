---
title: prop || attr
tags:
  - jquery
url: 167.html
id: 167
categories:
  - 前端开发
date: 2016-11-16 11:22:12
---

自 jquery 1.6.2 添加了一个新的用法，就是 prop()，很多人分不清 prop 跟 attr 两个方的的用途，因为习惯了使用 attr 所以基本上都是用 attr ，甚至于 input value 取值也用 attr

```javacript
$('input').attr('value');
```

而她的正确写法应该是：

```javascript
$('input').val();
//设置值
$('input').val(value);
```

prop 与 attr 的用法区别是： prop设置的是某元素固有的属性，而attr设置的是写在html标签上的自定义属性。 例子如下：

```javascript
<input type="checkbox" checked="checked" haha="hello" >
var v1 = $('input').prop("checked"); //返回true/false，是否被选中，随状态改变而改变
var v2 = $('input').attr("checked"); //返回"checked"，这是你设置在标签上的，不会变
var v3 = $('input').attr("haha"); //返回"hello"，自定义属性
var v4 = $('input').prop("haha"); //返回undefined，根本没有这个固有属性
```

取 checkbox 值的错误是自己亲身经历的，特此标注，还有其他 jquery 版本变更带来的问题，可以查看此片博文：[http://www.admin10000.com/document/6968.html](http://www.admin10000.com/document/6968.html)