---
title: 'git 问题处理'
tags:
  - git
url: 439.html
id: 439
categories:
  - 开发工具
date: 2017-04-20 17:03:10
---

## fatal: refusing to merge unrelated histories

#### 场景

本地创建项目，在打算向 github 提交的时候

    git init
    

然后线上创建 repo，使用

    git remote add origin git@github.com:callcter/branchname.git
    git add .
    git commit -a -m 'init commit'
    git pull origin master
    

这个时候报错：fatal: refusing to merge unrelated histories 原因是自 git 2.9 开始，--allow-unrelated-histories 选项需要手动添加 来自官方的说明：

> "git merge" used to allow merging two branches that have no common base by default, which led to a brand new history of an existing project created and then get pulled by an unsuspecting maintainer, which allowed an unnecessary parallel history merged into the existing project. The command has been taught not to allow this by default, with an escape hatch --allow-unrelated-histories option to be used in a rare event that merges histories of two projects that started their lives independently.

所以指令应该是：

    git pull origin master --allow-unrelated-histories

## 修改 .gitignore 不生效

原因是.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交

```
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```