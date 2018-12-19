---
title: Vue 图片引用
tags:
  - vue
url: 437.html
id: 437
categories:
  - 前端开发
date: 2017-04-17 15:30:52
---

### 页面图片

    data () {
        return {
            img: require('path/to/your/source')
        }
    }
    

然后在template中

    <img :src="img" />
    

### 背景图片

    data () {
        return {
            img: require('path/to/your/source')
        }
    }
    
    <div :style="{backgroundImage: 'url(' + img + ')'}"></div>
    

或者直接在css中定义

    background-image: url('path/to/your/source');