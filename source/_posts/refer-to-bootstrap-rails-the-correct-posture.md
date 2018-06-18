---
title: Rails 中引用 bootstrap 的正确姿势
tags:
  - ruby
  - rails
url: 202.html
id: 202
categories:
  - 后台开发
date: 2017-02-20 15:15:16
---

1、Gem 中添加

gem 'sass-rails', '~> 5.0' 
gem 'bootstrap-sass'

2、样式引用 新建一个 bootstrap_overrides.scss 文件，文件中引用 bootstrap

@import 'bootstrap-sprockets'; 
@import 'bootstrap';

在 application.scss 文件的 require 列表中将其放在首位，原因是 css 的优先级，后面的优先级高于前面的，为了定制，所以要将 bootstrap 的样式放在最前面 3、脚本文件引用 application.js 的 require 列表中添加 bootstrap