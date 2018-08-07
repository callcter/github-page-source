---
title: 网页图标（或图片）与文字共同存在时垂直居中
tags:
  - css&amp;3
url: 197.html
id: 197
categories:
  - 前端开发
date: 2017-02-17 16:21:30
---

网页中经常用到图标与文字的组合，如果图标与文字字体大小相同，基本上可以保持高度一致，然后垂直居中，对标准化研究较深的，推荐张鑫旭师兄的一篇文章： [以20像素为基准的CSS网页布局实践分享](http://www.zhangxinxu.com/wordpress/2016/03/css-layout-base-20px/) 但是，还有一些时候，设计是不想让你好过，设计各种超大图标加小文字的组合，比如 ![](http://blog.dreamser.com/wp-content/uploads/2017/02/WX20170217-161101@2x-1024x84.png) 或者按钮，为了凸显图标 ![](http://blog.dreamser.com/wp-content/uploads/2017/02/WX20170217-161205@2x.png) 为此，需要用别的方式处理，先上代码：

```scss
.iconBox{
  display: inline-block;
  .iconBox_icon{
    display: table-cell;
    vertical-align: middle;
  }
  .iconBox_text{
    display: table-cell;
    padding-left: 5px;
    vertical-align: middle;
  }
}
```

方法很简单，就是使用 display: table-cell，因为在 table 中的 vertical-align: middle 才能产生明确的效果，实际使用中：

```html
<div class='input-group'>
  <input type='text' class='form-control'>
  <div class='input-group-addon'>
    <div class='iconBox'>
      <i class='fa fa-plus iconBox_icon'></i>
      <span class='iconBox_text'>添加</span>
    </div>
  </div>
</div>
```

![](http://blog.dreamser.com/wp-content/uploads/2017/02/WX20170217-162037@2x-1024x77.png)