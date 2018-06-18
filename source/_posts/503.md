---
title: rails 线上部署 js 查错
tags:
  - rails
url: 503.html
id: 503
categories:
  - 后台开发
date: 2017-08-10 12:19:35
---

到项目目录下

    rails c
    

    JS_PATH = "app/assets/javascripts/**/*.js"; 
    Dir[JS_PATH].each do |file_name|
      puts "\n#{file_name}"
      puts Uglifier.compile(File.read(file_name))
    end