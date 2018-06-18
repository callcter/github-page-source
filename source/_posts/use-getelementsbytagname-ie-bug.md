---
title: IE中使用getElementsByTagName Bug
tags:
  - javascript
url: 75.html
id: 75
categories:
  - 前端开发
date: 2016-05-19 06:21:39
---

错误代码：

var div = document.createElement('div');
div.className = 'address-box';
div.innerHTML = // <p class="address-tip">请选择安装地点</p>
  '<ul class="address-tab">'+
  '<li>省</li>'+
  '<li>市</li>'+
  '<li>县区</li>'+
  '</ul>'+
  '<ul class="address-list"></ul>'+
  '<ul class="address-list"></ul>'+
  '<ul class="address-list"></ul>';
var tabs = getElementsByClassName(div,'address-tab')\[0\].getElementsByTagName('li');
document.body.appendChild(div);

这段代码在chrome中运行时没有问题的，而且之所以将代码写成这样的顺序就是为了减少渲染，但是代码在IE中运行却会出现问题，IE文档解释：

> _This problem occurs because the GetElementsByTagName method returns an XmlNodeList collection that registers listeners on the NodeInserted and the NodeRemoved events. For example, when you call the GetElementsByTagName method ten times, the NodeInserted and the NodeRemoved events have ten listeners. Therefore, when you call the GetElementsByTagName method multiple times, the process of inserting and removing nodes is delayed._

原因是索要获取的元素未加入文档流。 改正：

document.body.appendChild(div);
var tabs = getElementsByClassName(div,'address-tab')\[0\].getElementsByTagName('li');

但是这样子后面的一些元素操作都刷新document，消耗了浏览器资源……