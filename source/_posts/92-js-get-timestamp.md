---
title: Js 获取当天零点时间戳
tags:
  - javascript
url: 447.html
id: 447
categories:
  - 前端开发
date: 2017-05-04 14:44:11
---

    var date = new Date();
    date.setHours(0);
    date.setMinutes(0);
    date.setSeconds(0);
    date.setMilliseconds(0);
    var timestamp = date.getTime();
    var unix_timestamp = Math.floor(date.getTime() / 1000);