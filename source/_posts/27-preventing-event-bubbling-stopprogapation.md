---
title: 阻止事件冒泡—stopProgapation
tags:
  - javascript
url: 78.html
id: 78
categories:
  - 前端开发
date: 2016-05-19 07:16:38
---

在开发中经常会遇到事件冒泡的处理，如点击别处隐藏菜单，正常的处理方法：

event.stopPropagation();

但是在IE8中并不支持，方法改写成这样：

function stopProgapation(e){
  e = e || window.event;
  if(e.stopPropagation){
    e.stopPropagation();
  }else{
    e.cancelBubble = true;
  }
}

在IE8中识别的是cancelBubble，默认为false，将其设置为true即可。 其中要注意的一点，传入方法的事件要做兼容处理，e=e||window.event，刚刚被坑了 T_T…