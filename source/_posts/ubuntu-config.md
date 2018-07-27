---
title: Ubuntu服务器基础配置
tags:
  - linux
categories:
  - 服务器
---

#### 用户配置

##### 修改root用户密码
```
passwd
```
##### 创建新用户
```
adduser newuser
vi /etc/sudoers
// 添加
newuser ALL=(ALL:ALL) ALL
```

#### 防火墙

```
// 查看 iptables 安装状态
whereis iptables
// 如果没有安装
apt-get install iptables
// 查看防火墙配置信息
iptables -L -n
// 配置规则
vi /etc/iptables.rules
// 复制下面的内容到文件中
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 443 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
-A INPUT -p icmp -m limit --limit 1/s --limit-burst 10 -j ACCEPT
-A INPUT -p icmp -m limit --limit 100/sec --limit-burst 100 -j ACCEPT
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT
// 退出文件并保存，运行指令使配置文件生效
iptables-restore < /etc/iptables.rules
// 防火墙开机启动
vi /etc/network/if-pre-up.d/iptables
// 添加以下内容
#!/bin/bash 
iptables-restore < /etc/iptables.rules 
# chmod +x /etc/network/if-pre-up.d/iptables #添加执行权限
```

#### ssh
```
// 备份配置文件
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
// 修改登录端口以及禁用root登录
Port 22 -> 2018
PermitRootLogin yes -> no
// 使用指令
/etc/init.d/ssh start // 启动
/etc/init.d/ssh stop // 停止
/etc/init.d/ssh restart // 重启
```

##### 免密登录
```
mkdir ~/.ssh
chmod 700 ~/.ssh
vi ~/.ssh/authorized_keys
// 在文件中添加 public key
chmod 644 ~/.ssh/authorized_keys
// 修改配置文件，将注释去掉
#RSAAuthentication yes
#PubkeyAuthentication yes
#AuthorizedKeysFile %h/.ssh/authorized_keys
// 关闭密码登录
PasswordAuthentication no
// 重启 ssh
```

#### nginx
```
apt-get install nginx
/etc/init.d/nginx start // 启动
/etc/init.d/nginx restart // 重启
/etc/init.d/nginx stop // 停止
/etc/nginx/conf.d/ //配置文件目录
nginx -t // 验证配置文件有无语法错误
sudo tail -n 200 -f /var/log/nginx/access.log // 追踪log
```

#### mysql

##### 安装
```
apt-get install mysql-server mysql-client libmysqlclient-dev
/etc/init.d/mysql start // 启动
/etc/init.d/mysql restart // 重启
```
##### 修改默认端口，防止攻击
```
port = 3306 -> 3680
/etc/mysql/mysql.conf.d/mysqld.cnf // 配置文件路径
```
##### 远程连接配置
```
mysql -u root -h localhost -p
use mysql;
grant all on *.* to 'newuser'@'%' identified by 'newpwd'; // %代表所有
flush privileges; // 刷新权限表，使配置生效
// 注释配置文件
# bind-address = 127.0.0.1
```

#### node

##### nvm
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```