---
title: 保留但禁用滚动条
tags:
  - javascript
url: 184.html
id: 184
categories:
  - 前端开发
date: 2016-12-19 15:29:06
---

在实现弹窗效果时，一般需要遮罩背景，此时就会遇到滚动条的问题。遮罩的定位：

position: fixed;
left: 0;
top: 0;
right: 0;
bottom: 0;

如果，页面的高度高于窗口高度，滚动条会出现，为了防止滚动条移动，可以使用：

overflow-y: hidden;

但是这种方式会在弹出遮罩的时候出现跳动，原因是没有滚动条的页面宽度比原来宽了。想到的另外一种方式是去除滚动的事件绑定：

!function () {
  var keys = { 37: 1, 38: 1, 39: 1, 40: 1 };
  function preventDefault(e) {
    e = e || window.event;
    if (e.preventDefault)
      e.preventDefault();
    e.returnValue = false;
  }
  function preventDefaultForScrollKeys(e) {
    if (keys\[e.keyCode\]) {
      preventDefault(e);
      return false;
    }
  }
  var oldonwheel, oldonmousewheel1, oldonmousewheel2, oldontouchmove, oldonkeydown, isDisabled;
  function disableScroll() {
    if (window.addEventListener) // older FF
      window.addEventListener('DOMMouseScroll', preventDefault, false);
    oldonwheel = window.onwheel;
    window.onwheel = preventDefault; // modern standard
    oldonmousewheel1 = window.onmousewheel;
    window.onmousewheel = preventDefault; // older browsers, IE
    oldonmousewheel2 = document.onmousewheel;
    document.onmousewheel = preventDefault; // older browsers, IE
    oldontouchmove = window.ontouchmove;
    window.ontouchmove = preventDefault; // mobile
    oldonkeydown = document.onkeydown;
    document.onkeydown = preventDefaultForScrollKeys;
    isDisabled = true;
  }
  function enableScroll() {
    if (!isDisabled) return;
    if (window.removeEventListener)
      window.removeEventListener('DOMMouseScroll', preventDefault, false);
    window.onwheel = oldonwheel; // modern standard
    window.onmousewheel = oldonmousewheel1; // older browsers, IE
    document.onmousewheel = oldonmousewheel2; // older browsers, IE
    window.ontouchmove = oldontouchmove; // mobile
    document.onkeydown = oldonkeydown;
    isDisabled = false;
  }
  window.scrollHanlder = {
    disableScroll: disableScroll,
    enableScroll: enableScroll
  };
}();

但是这样又遇到了新的问题，如果在弹出中的内容需要滚动，这种方式导致所有的滚动都失效，所以需要给弹出的内容再单独加上滚动事件监听：

function enableScroll(e){
  e.stopPropagation();
  e.cancelBubble = false;
  var obj = $('.imgBrowser\_\_box\_inner').get(0);
  var delta = 0;
  if(e.wheelDelta){
    delta = e.wheelDelta/120; //IE、Opera
  }else if(e.detail){
    delta = -e.detail/3; //Mozilla
  }
  if($(obj).innerHeight() + $(obj).scrollTop() >= obj.scrollHeight){
    if(delta<0){
      e.preventDefault();
      return false;
    }
  }
  if($(obj).scrollTop() === 0){
    if(delta>0){
      e.preventDefault();
      return false;
    }
  }
  return false;
}
function removeWheelEvent(e){
  e.stopPropagation();
  e.preventDefault();
  e.cancelBubble = false;
  return false;
}

调用：

$(document).get(0).addEventListener('mousewheel',removeWheelEvent,false);
$('.imgBrowser\_\_box\_inner').get(0).addEventListener('mousewheel',enableScroll,false);
$(document).get(0).removeEventListener('mousewheel',removeWheelEvent,false);
$('.imgBrowser\_\_box\_inner').get(0).removeEventListener('mousewheel',enableScroll,false);

其中用了一些 jQuery 的方法 整理自：[https://www.zhihu.com/question/21865401](https://www.zhihu.com/question/21865401)