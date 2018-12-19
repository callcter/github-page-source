---
title: devise 登录方式修改（邮箱、用户名、手机号）
tags:
  - ruby
  - rails
  - devise
url: 535.html
id: 535
categories:
  - 后台开发
date: 2018-01-09 12:20:39
---

#### 单方式登录

选择登录方式前要保证 users 表中有这个字段 以修改登录方式为 username 为例

##### 一种方式是修改 User model，添加

    devise :database_authenticatable, :authentication_keys => [:username]
    

authentication_keys 中只能填写一种

##### 另一种方式修改 devise.rb 的配置项

    config.case_insensitive_keys = [:username]
    

我使用这种方式的时候不管用，还没找到原因

#### 多方式登录

同样的，选择登录方式前要保证 users 表中有这个字段 以允许 email、username两种方式登录为例

##### 1\. 在 User model 中添加虚拟属性

    attr_accessor :login
    

##### 2\. 修改允许参数，在 app/controllers/application_controller.rb 中添加

    before_action :configure_permitted_parameters, if: :devise_controller?
    protected
      def configure_permitted_parameters
        devise_parameter_sanitizer.permit(:sign_in, keys: [:login, :password])
      end
    

##### 3\. 修改 authentication_keys

同当方式登录，这里要修改为 login

##### 4\. 在 User model 中覆盖 find\_for\_database_authentication 方法

    protected
      def self.find_for_database_authentication warden_conditions
        conditions = warden_conditions.dup
        login = conditions.delete(:login)
        where(conditions).where(["lower(username) = :value OR lower(email) = :value", {value: login.strip.downcase}]).first
      end
    

调试时这里可以添加断点，如果不通过这里，可能就是 authentication_keys 修改没有生效

##### 5\. 修改前端传参

现在前端传递的参数应该修改为 login 跟 password