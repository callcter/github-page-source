---
title: package.json语法
tags:
  - node
url: 87.html
id: 87
categories:
  - 后台开发
date: 2016-05-30 09:08:29
---

 

version           严格匹配某个版本
>version          必须大于某个版本
>=version
<version
<=version
~version          大概匹配某个版本
^version          兼容某个版本
1.2.x             可以是1.2.0, 1.2.1等等，但不能是1.3.0
http://...        指定tarball的url地址
*                 任何版本都可以
""                同上
version1-version2 >=version1 <=version2
range1 || range2  满足range1 或range2
git://... git     地址
user/repo         同上
tag               指定某个tag的版本
path/path         本地包所有文件夹