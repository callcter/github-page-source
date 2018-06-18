---
title: jquery each 遍历退出
tags:
  - jquery
url: 505.html
id: 505
categories:
  - 前端开发
date: 2017-08-21 10:52:13
---

    return false; //相当于循环的 break
    return true; //相当于循环的 continue
    

所以判断元素存在的正确写法应该是这个样子：

    function isLiExist(id){
      var flag = true
      $('.workbench_select_list li').each(function(){
        if(parseInt(id) == parseInt($(this).data('id'))){
          flag = false
          return false
        }
      })
      return !flag
    }