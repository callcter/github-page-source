---
title: 使用 GIt 实现服务器自动部署
tags:
  - git
url: 464.html
id: 464
categories:
  - 服务器
date: 2017-07-08 13:22:03
---

### 1、线上服务器

    user git
    host 110.110.110.110
    

### 2、创建线上 git repo

    mkdir /var/repo //创建 git repo 的目录
    cd /var/repo
    sudo git init --bare glog.git
    

### 3、配置 git hooks

*   在 index.git 目录下有一个 hooks 目录，在目录中创建一个 post-receive 文件

    #!/bin/bash
    git --work-tree=/home/www/blog --git-dir=/var/repo/blog.git checkout -f
    

*   post-receive 是一个可执行文件，这里仅仅同步代码到目录下，还有其他指令也可以写进去
*   添加执行权限

    chmod +x post-receive
    

### 4、创建本地 git repo

*   创建完成后，关联远程库

    git remote add origin user@110.110.110.110:/var/repo/index.git
    

*   然后就可以执行其他指令，同步代码到线上了