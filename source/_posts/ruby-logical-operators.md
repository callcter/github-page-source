---
title: Ruby 逻辑运算符
tags:
  - ruby
url: 165.html
id: 165
categories:
  - 后台开发
date: 2016-11-15 10:48:49
---

刚开始 Rails 的学习，看项目源码的时候遇见一个 ||= 的语句：

@current\_user ||= session\[:user\_id\] && User.find(session\[:user_id\])

这个语句翻译过来就是：

if @current_user
   return @current_user
else
   if session\[:user_id\]
      @current\_user = User.find(session\[:user\_id\])
   else
      @current_user = nil
   end
   return @current_user
end

此处总结各符号用法： and 与 && 为和，区别是and优先级比&&低。 or 与 || 为或，not与!为非，区别均是前者优先级低于后者 &&=, !=, ||=这个比较灵活，可以认为它相当于+=,-=。 a &&= b即为a = a && b。可见Ruby比Java灵活很多。 Ruby的&&, ||与其它语言有些不同。 &&运算法则为：左边为false或nil时，直接分别返回false或nil，右边将不会运算。 左边不为false或nil时，返回右边的对象。 ||运算法则为：左边为false或nil时，返回右边的对象。 左边不为false或nil时，直接返回左边的对象，右边的不会运算。 几个例子：

puts false && "abc"      # => false
puts nil   && "abc"      # => nil

puts true  && "abc"      # => "abc"
puts "123" && "abc"      # => "abc"

puts false || "abc"      # => "abc"
puts nil   || "abc"      # => "abc"

puts true  || "abc"      # => true
puts "123" || "abc"      # => "123"

一个更深入的例子：

x ||= y
#相当于
x || x=y
#而不是
x = x||y
#区别在于如果x存在且不为空时不会执行任何操作，直接返回。
#还相当于
if defined? x
    x || x=y
else
    x = y
end

内容整理自：http://rubyer.me/blog/568/