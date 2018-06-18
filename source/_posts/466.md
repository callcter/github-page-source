---
title: canvas toDataURL()跨域问题
tags:
  - canvas
url: 466.html
id: 466
categories:
  - 前端开发
date: 2017-07-11 22:00:24
---

在使用 canvas toDataURL 方法时遇到这样的报错

    Uncaught DOMException: Failed to execute 'toDataURL' on 'HTMLCanvasElement': Tainted canvases may not be exported.
    //被污染的 canvas 不能导出
    

### 什么是被污染的 canvas？

不通过 CORS(cross-origin sharing 跨域资源共享) 虽然可以调用图片，但是这会污染 canvas。一旦 Canvas 被污染，你就无法读取其数据，如 toBlob、toDataURL 等方法都无法使用。这种机制可以避免未经许可拉取远程网站信息而导致的用户隐私泄露。

### 如何解决这个问题

一种方式是：在服务器中配置 Access-Control-Allow-Origin，各种语言都与自己配置的方式，这里不多说。 另一种方式是在前端解决： 原本的调用可能是

    var img = new Image()
    img.src = src
    

在配置 src 属性前加入

    img.crossOrigin = 'anonymous'
    

也可以解决跨域污染的问题