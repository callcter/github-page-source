---
title: jquery点击其他地方隐藏
tags:
  - jquery
url: 496.html
id: 496
categories:
  - 前端开发
date: 2017-07-17 14:49:22
---

这是在制作展开隐藏菜单时i遇到的一个问题，经过查找实践，这里介绍 3 种方法

#### 方法一

将隐藏事件绑定到 document 上，在 menu 上通过 stopPropagation 阻止事件冒泡

    $(document).on('click', '#stop', function(e){
      e.stopPropagation()
      alert('stop')
    })
    

测试地址：[点击其他地方隐藏实例](https://codepen.io/callcter/pen/XgQZQv "点击其他地方隐藏实例") 这种方法的缺陷：如果在其他元素也阻止事件冒泡，那么就不能够隐藏 menu 了，所以就有了方法二

#### 方法二

不使用 stopPropagation，而是通过判断是否是事件触发的元素来决定是否隐藏

    $(document).on('click', function(e){
      if ($(e.target).closest('.menu, #btn').length === 0) {
        $('.menu').hide();
      }
    })
    

测试地址：[点击其他地方隐藏方法二](https://codepen.io/callcter/pen/rwbKZb "点击其他地方隐藏方法二")

#### 方法三

通过一款插件使 jquery 只是 outside 事件 地址：[jquery outside events](http://benalman.com/projects/jquery-outside-events-plugin/ "jquery outside events")