---
title: CSS 控制输入框伸缩
tags:
  - css&amp;3
url: 210.html
id: 210
categories:
  - 前端开发
date: 2017-02-23 18:01:25
---

基本原理是判断 placeholder 是否显示：

input{
  width: 200px;
  &:placeholder-shown{
    width: 100px;
  }
}

:placeholder-shown 是很前卫的一个伪类，当前的兼容性较差，慎用： ![](http://blog.dreamser.com/wp-content/uploads/2017/02/WX20170223-174009@2x-1024x229.png) ![](http://blog.dreamser.com/wp-content/uploads/2017/02/WX20170223-174329@2x-1024x230.png)