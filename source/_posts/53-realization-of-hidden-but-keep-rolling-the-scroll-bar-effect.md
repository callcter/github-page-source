---
title: 隐藏滚动条但是保留滚动效果的实现
tags:
  - css&amp;3
url: 182.html
id: 182
categories:
  - 前端开发
date: 2016-12-16 16:53:41
---

关键点：滚动条的宽度大概是 20px 我们的实现是将滚动条覆盖

<div class="box_outer">
   <div class="box_inner">
      <div class="box">
          ......
      </div>
   </div>
</div>

样式：

.box_outer{
  position: relative;
  overflow: hidden;
  width: 300px;
  height: 500px;
}
.box_inner{
  position: absolute;
  left: 0;
  right: -20px;  //覆盖滚动条
  overflow-x: hidden;
  overflow-y: scroll;
}
//跟外层容器大小一致
.box{
  width: 300px;
  height: 500px;
}

对于使用 Webkit 内核的浏览器来说还可以使用这样的方式：

.element::-webkit-scrollbar {
  display: none;
}