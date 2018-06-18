---
title: Rails按需加载静态文件
tags:
  - ruby
  - rails
url: 186.html
id: 186
categories:
  - 后台开发
date: 2017-01-03 20:08:58
---

Rails 使用 Assets Pipeline 管理静态文件，为了减少请求次数，一般会将所有的静态文件压缩为一个 application 文件，在页面中仅仅引入 application.css 文件跟 application.js 文件。但是在有时候，可能一些文件容量的比较大的静态文件仅在一个或者两个页面使用，或者两个文件之间存在冲突，这个时候需要控制在不同控制器下使用不同的静态文件，也就是按需加载。 在初始化的项目中的 application.js(css) 文件中存在：

require_tree .

意思是引入路径下的所有文件，需要将其注释。 然后将一些需要单独引入的文件单独建一个文件，可能为 other.js(css)，文件的作用和写法跟 application.js(css) 相同，作为一个压缩合并的文件。然后在 config->initializers->assets.rb 文件中添加新的文件：

//原本
Rails.application.config.assets.precompile += %w( app.js )
//新的
Rails.application.config.assets.precompile += %w( app.js other.js )

最后在 include_tag 前添加引用条件，如：

\- if params\[:controller\] == 'other'
      = javascript\_link\_tag 'other', media: 'all'