---
title: Ruby 中英文混合截取字符串截取
tags:
  - ruby
url: 455.html
id: 455
categories:
  - 后台开发
date: 2017-05-12 16:35:54
---

其基本原理是使用字节来计量长度，其中参数 maxsize 代表的是字节，英文字符一个字符是一个字节，而汉字是三个。这种方式是比较准确的一种中英文混合截取的方法。还有别的方法欢迎讨论

def truncate\_u(unicode\_string, maxsize)
  return unicode\_string if unicode\_string.blank?
  size = 0
  s = ""
  unicode\_string.each\_char do |c|
    c\_size = c.bytes.to\_a.length
    size += c_size
    if size >= maxsize
      s.concat(c)
      if s != unicode_string
        s = s.slice(0, s.length - 1).concat('...')
      end
      break
    else 
      s.concat(c)
    end  
  end 
  s
end