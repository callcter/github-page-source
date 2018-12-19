---
title: 网页截图实现方案
tags:
  - canvas
url: 159.html
id: 159
categories:
  - 前端开发
date: 2016-10-10 12:48:12
---

方案实现需要借助一个 js 插件：html2canvas 地址：[https://html2canvas.hertzen.com/](https://html2canvas.hertzen.com/) 使用方法很简单，网页中加载html2canvas，然后

html2canvas(document.body, {
  onrendered: function(canvas) {
    document.body.appendChild(canvas);
  }
});

onrendered 是截完图之后的 callback ，canvas是生成的 canvas 截图，通过 Canvas 的 toDataUrl 方法获得 base64 串，finished