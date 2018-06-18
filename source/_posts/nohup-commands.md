---
title: nohup commands &
tags:
  - Linux
url: 511.html
id: 511
categories:
  - 服务器
date: 2017-09-11 11:20:26
---

有些进程需要一直启动，占用终端窗口，如

    redis-server
    

但是并不想让他们占用窗口，所以就用到了这个指令

    nohup redis-server &
    

指令的作用是使进程在后台运行，不占用终端

#### 关闭

如果没有终端，那么怎么关闭进程呢，答案是站到 pid，然后 kill

    ps aux | grep redis
    kill pid
    

#### 扩展

还有一些程序自带守护进程的指令，如 php-fpm

    sudo php-fpm -D
    

也可以不占用终端