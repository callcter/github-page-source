---
title: 迁移到腾讯云
---

这两天腾讯云活动，新用户“2核-4G-5M-50G-3年”只要998，阿里云的服务器也快到期了，赶紧薅个羊毛迁移过去吧。

## 基本配置

```
sudo apt-get update  # 更新软件列表
sudo apt-get upgrade  # 更新软件
sudo apt-get install zsh vim git
sudo apt-get install nginx mariadb-server mongodb
```

# ZSH

```
cat /etc/shells  # 当前可用 shell
echo $SHELL  # 当前使用的 shell
sudo vim /etc/passwd  # 用户配置文件，可以修改所有用户 shell
```

- 找到当前用户修改要用的 shell，退出重新登录，将启用新的 shell
- zsh 启启动时如果当前用户目录下没有 .zshrc 文件，将会询问，我选择用社区最流行版本

# Vim

# SSH

### 生成 Key

```
ssh-keygen  # 选择默认项
cat .ssh/id_ras.pub  # 打印 PublicKey，可以复制到 github 之类的地方使用
```

### 添加本地 PublickKey 到服务器

```
vim .ssh/authorized_keys

# 添加内容

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDThKBl2f6TzTK06WQZONG85u7DbocECmA0ukW09wrB/S3wgmilp2DfOtuaCIkJA0ZqW7cUcuThtJwwyfp+kaWVnD/WHAyuvBqFBQHf9Rk5rc+lPhmvKvee8EVwKEnJoUETV0j3Y8ZGbt/4VQSYIfWLPiyWg0E1xQ0VStIMFX64GqcaVCnagEtW2mfF6ofZKtXRl2B4+4JPfRvnbIbh4e4CsOHgjdDEq63he4tneZ7rS3GBkFsv3h2Q0Ln6uyn1Df5G6TiKq5fqbKhWfkjxXB3b+f/2wKInKak0/2SX09U+VDifd0Oz/ORzaaX/wpoXMo6bhAuOdK/kQCwdg/s2oTHX callcter@liuhangdeMacBook-Pro.local
```

### 配置文件修改——免密登录

```
sudo vim /etc/ssh/sshd_config

# 修改内容

Port xxx  # 修改陌生端口后
AuthorizedKeysFile .ssh/authorized_keys  # 公钥的配置目录
PubkeyAuthentication yes  # 可以使用公钥登录
PasswordAuthentication no  # 关闭密码登录

# 重启生效
sudo service sshd restart
```

# Nginx

```
sudo vim /etc/nginx/nginx.conf  # 配置文件
/etc/nginx/conf.d/  # 反向代理配置目录
```

# MySQL

```
sudo mysql_secure_installation
sudo mysql -uroot -p
```

在 MySQL 下执行
```
create user 'username'@'%' identified by 'password';  # 创建新的用户，%表示可以从任意远程主机登陆
grant all on *.* to 'username'@'%';  # 授权，all *.* 表示对所有数据库及表拥有所有权限
```

```
sudo vim /etc/mysql/mariadb.conf.d/50-server.cnf

# 50-server.cnf 修改配置
port your_port
bind-address 0.0.0.0

systemctl restart mysql
```

#### NVM & NodeJs

```
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.0/install.sh | bash

vim .zshrc

# 添加如下内容

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

source .zshrc

nvm ls-remote
nvm install v12.12.0
```

#### Python

```

```


# code-push-server 配置

# Frp 内网穿透配置