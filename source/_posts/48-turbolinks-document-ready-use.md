---
title: Turbolinks中document.ready的使用
tags:
  - ruby
  - rails
url: 169.html
id: 169
categories:
  - 后台开发
date: 2016-11-17 10:41:49
---

Turbolinks的基本原理是：使用pushState，从服务器加载整个网页，并替换当前加载的DOM中的<title>和<body>节点。默认情况下，页面所有的链接都遵循这个规则。所以 document.ready 仅仅会在页面初始加载的时候执行一边，导致的问题是在许多页面中需要的 ready 时需要执行的 js 没法执行，所以需要添加事件监听。

$(document).on('ready page:load',function(){
   initPage();
})

而在 Rails5 中，也就是 Turbolinks5 事件需要修改为 turbolinks:load 。