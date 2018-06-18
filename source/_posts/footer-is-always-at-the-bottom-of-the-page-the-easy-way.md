---
title: footer 始终位于页面底部的简单方式
tags:
  - css&amp;3
url: 206.html
id: 206
categories:
  - 前端开发
date: 2017-02-23 17:13:46
---

页面布局：

<body>
  <header></header>
  <div class="wrapper"></div>
  <footer></footer>
</body>

样式：

html, body{
  width: 100%;
  min-height: 100%;
}

body{
  position: relative;
  padding-bottom: 80px;
}

footer{
  bottom: 0;
  left: 0;
  right: 0;
  position: absolute;
  height: 60px;
}