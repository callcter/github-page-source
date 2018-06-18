---
title: Ruby开发环境配置
tags:
  - ruby
url: 43.html
id: 43
categories:
  - 后台开发
date: 2016-05-04 06:43:08
---

1、Ruby介绍 2、安装(Mac) (1)预备 安装Xcode，需要打开一次并且同意开发协议 (2)安装Homebrew(MacOs工具包管理器)

ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

(3)安装Ruby 可以通过Homebrew来安装

brew install ruby

也可以通过rvm安装，rvm是ruby的版本管理器 安装rvm

curl -sSL https://get.rvm.io | bash -s stable

然后通过rvm安装ruby

rvm install

3、Hello World! 4、Ruby on Rails