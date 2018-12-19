---
title: Rails flash 定制
tags:
  - ruby
  - rails
url: 216.html
id: 216
categories:
  - 后台开发
date: 2017-02-24 18:39:03
---

// layouts/application.haml
= render 'shared/flash'

// shared/_flash.haml
\- flash.each do |type, message|
  .flash{class: "#{(type=='success' || type == 'notice')? 'success': 'error'}"}
    .container
      \- if type == 'success' || type == 'notice'
        %i.icon.iconfont.icon-tubiao19
        = message
      \- else
        %i.icon.iconfont.icon-cha
        = message

      .flash__close
        %i.icon.iconfont.icon-x

\- if flash
  // 自动隐藏
  :javascript
    var timer = setTimeout(function(){
      $('.flash').fadeOut('fast');
      clearTimeout(timer)
    }, 5000)