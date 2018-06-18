---
title: Canvas 制作仿脉脉企业名称 logo
tags:
  - canvas
url: 157.html
id: 157
categories:
  - 前端开发
date: 2016-10-10 12:41:35
---

<canvas id='nametoimg' width='200' height='200'></canvas>

  if(document.getElementById('nametoimg')){
    var text = "拙朴";
    var textArr = text.split('');
    var c = document.getElementById('nametoimg');
    var ctx = c.getContext('2d');
    ctx.fillStyle='#fff';
    /\*\* 圆
    ctx.beginPath();
    ctx.arc(100,100,100,0,Math.PI*2,true);
    ctx.closePath();
    ctx.fill(); */
    ctx.fillRect(0,0,200,200);
    ctx.fillStyle="#000";
    switch(textArr.length){
      case 2:
        ctx.font="90px Arial";
        ctx.fillText(textArr\[0\],10,130);
        ctx.fillText(textArr\[1\],105,130);
        break;
      case 3:
        ctx.font="60px Arial";
        ctx.fillText(textArr\[0\],10,120);
        ctx.fillText(textArr\[1\],70,120);
        ctx.fillText(textArr\[2\],130,120);
        break;
      case 4:
        ctx.font="65px Arial";
        ctx.fillText(textArr\[0\],30,85);
        ctx.fillText(textArr\[1\],110,85);
        ctx.fillText(textArr\[2\],30,160);
        ctx.fillText(textArr\[3\],110,160);
        break;
    }
    console.log(c.toDataURL());
  }

制作原理非常简单，使用 Canvas 的 fillText 方法进行文字排版，然后用 toDataUrl 生成图片的 base64 串，即可以上传七牛 Ps 1、我这里只做了2、3、4个字的排版，可以自由拓展 2、注册企业都有一个简称，可以从一些官方渠道获得