---
title: ThinkPHP 5 入门
tags:
  - php
  - thinkphp
url: 195.html
id: 195
categories:
  - 后台开发
date: 2017-02-10 11:00:42
---

年假期间开始准备重构个人站点, 纠结再三还是决定继续使用 TP 作为主站的后台框架, 当前最新版本是 5, 所以顺手升级到 5, 支持国产, 希望团队能够做得更好. 常用的文档都在看云, 不得不说, 国内的许多产品都做得很不错 完全开发手册 [http://www.kancloud.cn/manual/thinkphp5](http://www.kancloud.cn/manual/thinkphp5) 免费 其他的如入门手册 [http://www.kancloud.cn/thinkphp/thinkphp5_quickstart](http://www.kancloud.cn/thinkphp/thinkphp5_quickstart) 是需要购买的, 从 10 涨到现在的 20, 我也小小的支持了一下

### 环境

我尝试在 Mac 上配置了一下 Apache+PHP, 两者在 Mac 上都自带, 但是遇到一次 bug, 怎么都找不到问题, 索性自己重装了一次, 有一篇很不错的参考文章: [在 MacOS Sierra 上安装 Apache 和多个版本的 PHP](https://segmentfault.com/a/1190000007951274)

### 安装

三种方式: 官网下载, composer, git 我选择的是 composer