---
title: 修改 SSH 默认端口号 GIt 的配置
tags:
  - git
url: 470.html
id: 470
categories:
  - 开发工具
date: 2017-07-12 10:43:32
---

### 方法一：直接修改 URL 为 ssh:// 开头

    git remote set-url ssh://git@domain.com:xxx/route_to_repo/reponame.git
    

### 方法二：修改本地配置文件

mac 下

    vim ~/.ssh/config
    

添加配置

    host newdomain
    hostname domain.com //别名，可以不加
    port xxx