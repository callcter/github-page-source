---
title: Rails 分页插件 will_paginate
tags:
  - ruby
  - rails
url: 214.html
id: 214
categories:
  - 后台开发
date: 2017-02-24 12:03:04
---

安装 在 gemfile 中添加

gem 'will_paginate'

使用

//后台
@datas = Data.where(user\_id: 1).paginate(:page => params\[:page\], :per\_page => 10)
//模板(haml)
\- @datas.each do |data|
= will_paginate @datas //添加分页样式

配置 在 config/initializers 目录下创建 will_paginate.rb 文件

WillPaginate::ViewHelpers.pagination_options\[:class\] =  "yourclass"
WillPaginate::ViewHelpers.pagination\_options\[:previous\_label\] =  "前一页"
WillPaginate::ViewHelpers.pagination\_options\[:next\_label\] =  "后一页"
\# inner_window  控制显示当前页临近的多少个链接 ，默认是4
WillPaginate::ViewHelpers.pagination\_options\[:inner\_window\] =  2
\# outer_window 控制显示首/末页临近的多少个链接，默认是1
WillPaginate::ViewHelpers.pagination\_options\[:outer\_window\] =  0
\# 这个参数是用来设置页码之间 的分隔符的，用空格或者（|）或者其他的都可以
WillPaginate::ViewHelpers.pagination_options\[:separator\] =  0
\# 如果是false的时候，只显示上一页和下一页 (默认是 true)
WillPaginate::ViewHelpers.pagination\_options\[:page\_links\] = true
\# 这个参数是用来我们点击页码连接的时候传递的参数的名称，一般不用改动
WillPaginate::ViewHelpers.pagination\_options\[:param\_name\] = :page

分别是样式、前后页的中文