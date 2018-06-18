---
title: 'Sorry, you can''t use Pry without Readline or a compatible library.'
tags:
  - ruby
url: 498.html
id: 498
categories:
  - 后台开发
date: 2017-07-24 15:55:27
---

报错原因： 更新了 readline，而 ruby 是依赖 readline 的，所以需要重新编译 ruby。 我用的是 rvm 来管理 ruby，编译方法：

    //替换成自己使用的版本
    rvm reinstall ruby-2.3.1
    

如果中间报错，再重新运行一次！

#### Ps.

更新完后还可能遇到报错：

    undefined method `activate_bin_path' for Gem:Module (NoMethodError)
    

需要我们更新 gem 和 bundler

    gem update --system
    gem update bundler
    

然后在项目中再运行一下：

    bundle install
    

ok