---
title: Wordpress 访问加速配置
tags:
  - php
  - wordpress
url: 507.html
id: 507
categories:
  - 后台开发
date: 2017-08-25 10:36:24
---

#### 去掉不用的系统自带插件

如 embed、emoji 插件：Disable Embeds

    wp_deregister_script('wp-embed');
    

#### 去掉静态资源后的版本号

Remove Query Strings