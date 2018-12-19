---
title: JavaScript获取图片尺寸
tags:
  - javascript
url: 412.html
id: 412
categories:
  - 前端开发
date: 2017-04-07 18:06:34
---

在日常开发场景中，会有时候遇到根据图片尺寸来处理的问题，如全屏查看图片时图片的位置。

### 图片的高度小于屏幕高度

这种情况需要图片垂直居中，可以使用相对定位（可以使用 margin 来居中显示）跟 translate 结合，完全不需要 js 配合：

    position: relative;
    top: 50%;
    translateY: -50%; //这里不考虑多浏览器兼容
    

其他方法都需要知道图片的尺寸。

### 图片的高度大于屏幕高度

图片不存在垂直居中的问题，但是两种情况需要知道图片的尺寸。

### DOM Image 对象

Image 对象代表嵌入的图像。 标签每出现一次，一个 Image 对象就会被创建。

#### Image 对象的属性

除了id、name、src、alt、width、height、border、className、title等标准属性外，下面列出Image对象的其他重要属性： align 设置或返回与内联内容的对齐方式。可选值：left|right|top|middle|bottom complete 返回浏览器是否已完成对图像的加载。如果加载完成，则返回 true，否则返回 fasle。 hspace 设置或返回图像左侧和右侧的空白。hspace 属性可设置或返回图像的左边缘和右边缘的空白。hspace 和 vspace 属性可与 align 一同使用，来设置图像与周围文本的距 isMap 返回图像是否是服务器端的图像映射。 longDesc 设置或返回指向包含图像描述的文档的 URL。 lowsrc 设置或返回指向图像的低分辨率版本的 URL。 useMap 设置或返回客户端图像映射的 usemap 属性的值。 vspace 设置或返回图像的顶部和底部的空白。

#### 事件句柄

onerror 在装载图像的过程中发生错误时调用的事件句柄。 onabort 当用户放弃图像的装载时调用的事件句柄。 onload 当图像装载完毕时调用的事件句柄。

### 使用 Image 对象

在 JavaScript 中创建一个 Image 对象，给他的 src 赋值，就可以获取他的 width、height 等属性，这正是我们需要的。为了确定图片已经加载完，再使用 onload 事件，在回调中执行后续操作。

    var img = new Image();
    img.src = "http://dreamser.com/logo.png";
    img.onload = function(){
      console.log(img.width);
    }