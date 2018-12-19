---
title: 使用Guard-livereload实现浏览器自动刷新
tags:
  - gurad
  - livereload
url: 96.html
id: 96
categories:
  - 前端开发
date: 2016-06-30 02:50:44
---

1、安装Guard

gem install guard

2、安装guard-livereload 可以使用Gemfile安装

group :development do
  gem 'guard-livereload', '~> 2.5', require: false
end

或者直接

gem install guard-livereload

3、配置Guardfile文件

bundle exec guard init

将生成Guardfile，在文件中写入文件的监听项

guard 'livereload' do
  watch(%r{app/views/.+\\.(erb|haml|slim)$})
  watch(%r{app/helpers/.+\\.rb})
  watch(%r{public/.+\\.(css|js|html)})
  watch(%r{config/locales/.+\\.yml})
  # Rails Assets Pipeline
  watch(%r{(app|vendor)(/assets/\\w+/(.+\\.(css|js|html|png|jpg))).*}) { |m| "/assets/#{m\[3\]}"  }
end

4、配置浏览器 使用chrome安装livereload插件，暂时我只用过chrome，其他如firefox、Safari也有各自的配置，请自行查看 5、开启浏览器自动刷新 (1)启动guard

bundle exec guard start

(2)开启服务器

bundle exec rails s

(3)打开浏览器，开启新的标签，点击右上角插件栏中的livereload的图标，中间圆点变黑，刷入访问地址，一般为localhost:3000，端口号可以自己配置 之后修改文件保存后浏览器就能自动刷新，对于前端的同学用处极大，再也不用一遍一遍的刷新浏览器了\\(^o^)/~，当然，这是使用rails开发的配置，基于node的话可以使用gulp browser-sync实现，有专门的文章介绍，不过以sass来说，node-sass的实现比起ruby下的实在难用