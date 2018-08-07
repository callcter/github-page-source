---
title: Centos7 修改 SSH 端口号
tags:
  - centos7
  - ssh
url: 468.html
id: 468
categories:
  - 服务器
date: 2017-07-12 10:34:38
---

    vim /etc/sshd_config
    

修改 Port 为自己要设置的端口号

    service sshd restart