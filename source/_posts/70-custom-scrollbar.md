---
title: 自定义滚动条
tags:
  - css&amp;3
url: 368.html
id: 368
categories:
  - 前端开发
date: 2017-03-27 17:41:41
---

Webkit 浏览器：

```css
/* 设置垂直滚动条的宽度和水平滚动条的高度 */
::-webkit-scrollbar{
  width: 8px;
  height: 8px;
}
/* 设置滚动条的滑轨 */
::-webkit-scrollbar-track {
  background-color: #ddd;
}
/* 滑块 */
::-webkit-scrollbar-thumb {
  background-color: rgba(0, 0, 0, 0.6);
  border-radius: 4px;
}
/* 滑轨两头的监听按钮 */
::-webkit-scrollbar-button {
  background-color: #888;
  display: none;
}
/* 横向滚动条和纵向滚动条相交处尖角 */
::-webkit-scrollbar-corner {
  /\*background-color: black;\*/
}
```

插件推荐： mCustomScrollbar