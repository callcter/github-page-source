---
title: git branch常用指令
tags:
  - git
url: 173.html
id: 173
categories:
  - 开发工具
date: 2016-11-30 10:22:37
---

1、查看本地分支

git branch

2、查看远程分支

git branch -a

3、创建分支

git branch branch_name

4、切换分支

git checkout branch_name
//创建并切换
git checkout -b branch_name

5、删除本地分支

git branch -d branch_name

6、提交到远程分支

git push origin remote\_branch\_name
//如果远程分支不存在，会创建新的远程分支