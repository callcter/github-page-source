---
title: Markdownpad——在windows定制自己的markdown编辑器
tags:
  - markdown
url: 70.html
id: 70
categories:
  - 开发工具
date: 2016-05-17 09:46:56
---

Markdownpad是一款markdown编辑器，有windows版本。其包含两个版本，免费版和付费的Pro版，Pro版拥有诸多功能。 条件允许的同学可以选择支持正版，同时也有破解版以及一些开放的注册码，可以自己找以下。 在这里分享一些定制的方法。 1、在HTML导出目录 工具>选项>高级>HTML Head编辑器，在其中插入js代码：

<script>
    document.addEventListener("DOMContentLoaded", function() {
        // 生成目录列表
        var outline = document.createElement("ul");
        outline.setAttribute("id", "outline-list");
        outline.style.cssText = "border: 1px solid #ccc;";
        document.body.insertBefore(outline, document.body.childNodes\[0\]);
        // 获取所有标题
        var headers = document.querySelectorAll('h1,h2,h3,h4,h5,h6');
        for (var i = 0; i < headers.length; i++) {
            var header = headers\[i\];
            var hash = _hashCode(header.textContent);
            // MarkdownPad2无法为中文header正确生成id，这里生成一个
            header.setAttribute("id", header.tagName + hash);
            // 找出它是H几，为后面前置空格准备
            var prefix = parseInt(header.tagName.replace('H', ''), 10);
            outline.appendChild(document.createElement("li"));
            var a = document.createElement("a");
            // 为目录项设置链接
            a.setAttribute("href", "#" + header.tagName + hash)
            // 目录项文本前面放置对应的空格
            a.innerHTML = header.textContent;
            a.style.marginLeft = (prefix-1)*20 + 'px';
            outline.lastChild.appendChild(a);
        }

    });

    // 类似Java的hash生成方式，为一段文字生成一段基本不会重复的数字
    function _hashCode(txt) {
         var hash = 0;
         if (txt.length == 0) return hash;
         for (i = 0; i < txt.length; i++) {
              char = txt.charCodeAt(i);
              hash = ((hash<<5)-hash)+char;
              hash = hash & hash; // Convert to 32bit integer
         }
         return hash;
    }
</script>

  然后在使用的样式文件中添加目录的样式，工具>选项>样式表>编辑：

#outline-list{
  position: fixed;
  left: 50%;
  margin-left: -650px;
  top: 50px;
  width: auto;
  height: auto;
  padding: 10px;
}
#outline-list li{
  list-style: none;
}
#outline-list li a{
  text-decoration: none;
  padding: 2px 5px;
}
#outline-list a:hover{
  background: #4183C4;
  color: #fff;
}

这里是随滚动条移动的侧边栏目录，也可以自己定制其他样式。