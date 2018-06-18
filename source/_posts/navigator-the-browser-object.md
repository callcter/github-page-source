---
title: Navigator--浏览器对象
tags:
  - javascript
url: 175.html
id: 175
categories:
  - 前端开发
date: 2016-12-06 14:16:05
---

最近开发项目时遇见了PC端与移动端相差极大的页面效果，无奈，只能考虑通过 JS 识别访问端的浏览器来作区分处理显示不同的效果，这里记录一下浏览器对象 navigator 的一些属性：

navigator.appCodeName;//返回与浏览器相关的内部代码名，都为Mozilla
navigator.appName;//返回浏览器正式名称，均为Netscape
navigator.appVersion;//返回浏览器版本号
navigator.cookieEnabled;//返回浏览器是否启用cookie，true和false
navigator.geolocation;//返回地理定位信息(h5)
navigator.javaEnabled();//检测当前浏览器是否支持 Java，从而知道浏览器是否能显示 Java 小程序(IE,chrome返回true，firefox返回false)
navigator.language;//返回浏览器的首选语言
navigator.mimeTypes;//返回浏览器支持的Mime类型
navigator.msManipulationViewsEnabled;//仅支持IE，true
navigator.msMaxTouchPoints;//字面意思是最大的触摸点，IE为0，其他不支持
navigator.msPointerEnabled;//IE为true，其他不支持
navigator.onLine;//是否连接互联网，均返回true(未断网)
navigator.platform;//所在平台，返回win32
navigator.plugins;//返回浏览器插件集合
navigator.preference;//允许一个已标识的脚本获取并设置特定的 Navigator 参数
navigator.product;//浏览器产品名，返回gecko
navigator.systemLanguage;//获取系统语言，IE支持，返回zh-cn
navigator.userAgent;//判断浏览器类型
navigator.userLanguage;//返回操作系统的自然语言设置,IE支持，返回zh-cn

因为浏览器厂商不同或者版本不同，以上的属性也会有所不同，但是我们常用的 cookieEnabled、userAgent 等属性基本上不会发生变化。 还有一些有用的方法：

navigator.hasOwnProperty;//意思是是否支持属性，用法如下