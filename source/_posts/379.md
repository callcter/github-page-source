---
title: 七牛上传 token 策略
url: 379.html
id: 379
categories:
  - 建站解决策略
date: 2017-03-29 17:59:42
tags:
---

token 本身是有过期时间的，直接使用七牛封装的上传组件（选择文件或拖拽上传）时它本身会处理 token，也就是每次上传都会请求一次，所以我们自己定制上传时，也要按照这个方式来，先做一次 ajax 请求 token，然后将使用token的方法封装在 success 回调中