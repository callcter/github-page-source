---
title: Centos 7 服务器初始化用户配置
tags:
  - centos7
url: 443.html
id: 443
categories:
  - 服务器
date: 2017-04-24 18:14:54
---

### 使用 ssh 以 Root 身份登录

    ssh root@xxx.xxx.xxx.xxx
    

### 创建新用户，代替 root

服务器上一般禁忌直接使用 root 用户操作，所以我们需要创建一个拥有所有权限的新用户来代替 root 尽心所有服务器操作

    adduser username
    passwd username
    

输入密码，然后确认密码，创建用户完成。然后是更改用户分组为 wheel，使得用户拥有所有权限。

    gpasswd -a username wheel
    

会提示添加用户到组成功。

### 添加公钥认证

使用公钥的方式登录，可以避免每次都输入密码。首先在本地通过指令生成密钥

    ssh-keygen
    

生成的文件在 .ssh 目录下，包括 id\_rsa （私钥），id\_rsa.pub（公钥），请不要把自己的私钥发给任何人。打开公钥，复制。 回到服务器，切换到刚刚创建的用户下，创建 .ssh 目录，修改权限

    su - username
    mkdir .ssh
    chmod 700 .ssh
    

然后在 .ssh 目录下创建 authorized_keys 文件，将刚刚复制的公钥黏贴进去，然后修改文件权限

    chmod 600 .ssh/authorized_keys
    

### 禁止 root 登录，提高服务器安全性

    vim /etc/ssh/sshd_config
    

打开 ssh 的配置文件，将

    #PermitRootLogin yes
    

修改为

    PermitRootLogin no
    

还可以修改默认端口号，将 22 改为其他的，也可以有效的防范攻击

### 尝试新用户登录

此时已经配置好了服务器的用户和 ssh 登录，在本地另外打开一个终端，用新用户登录，切记，在尝试登陆成功之前千万不要把原本的 root 登录的终端关掉。

    ssh username@xxx.xxx.xxx.xxx
    

### 缩减登录指令

在本地环境打开文件

    vim .ssh/config
    

填写

    Host username
      HostName xxx.xxx.xxx.xxx
      Port xx
      User username
      IdentityFile ~/.ssh/id_rsa
    

然后就可以使用端的指令登录了

    ssh username
    

### 参考文章

[在CentOS 7初始服务器设置](https://www.howtoing.com/initial-server-setup-with-centos-7/) [Ruby on Rails 开发和生产环境搭建](http://wulfric.me/2016/03/ruby-on-rails-on-aliyun/)