---
title: '树莓派4B配置'
---

## 安装系统

#### 准备材料和工具

- 树莓派裸板，我的是树莓派4B 4G版
- 树莓派电源，5V/3A
- SD卡，闪迪64G
- 读卡器
- 系统，我选的是[Raspbian](https://www.raspberrypi.org/downloads/raspbian/)
- PC
- Win32diskimager

#### 安装

下载好想用的系统，在PC上使用Win32diskimager将系统刻入SD卡，刻录完毕将SD卡插入树莓派的卡槽，连接电源即可使用

## 开发环境

看自己的用途，配置不同的环境，我考虑做一个轻量的服务器，然后做一些爬虫工作，所以nginx、MySQL、MongoDB、Python、nodejs、这些都是要装的。

### 切换源镜像，更新软件

备份旧的源配置，切换为[清华](https://mirrors.tuna.tsinghua.edu.cn/help/raspbian/)的镜像，具体哪个镜像需要根据自己的系统来选择，我用的是Raspbian Buster
```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak  # 备份
sudo vim /etc/apt/sources.list  # 编辑
sudo apt-get update  # 更新软件列表
sudo apt-get upgrade  # 更新软件
```

我的sources.list为：
```
# 编辑 `/etc/apt/sources.list` 文件，删除原文件所有内容，用以下内容取代：
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main non-free contrib
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main non-free contrib

# 编辑 `/etc/apt/sources.list.d/raspi.list` 文件，删除原文件所有内容，用以下内容取代：
deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui
```

### 开发工具

- zsh
- vim
- git

```
sudo apt-get install zsh vim git
```

vim的配置不在这里说明了

#### 服务器

- nginx
- mysql
- mongodb

```
sudo apt-get install nginx mariadb-server mongodb
```

##### nginx

```
# 配置文件路径 /etc/nginx/sites-available/default
# 默认文件目录 /var/www/html/
```

#### python

#### nodejs

## 内网穿透

想要远程访问树莓派，我选择的方案是FRP+阿里云实现内网穿透

[Frp下载地址](https://github.com/fatedier/frp/releases)

### 阿里云服务器(Ubuntu 16.04)

**如果有开启防火墙，如iptables等，需要开放所需要使用的端口**

```
wget https://github.com/fatedier/frp/releases/download/v0.29.0/frp_0.29.0_linux_amd64.tar.gz
tar -zxvf frp_0.29.0_linux_amd64.tar.gz
sudo mv frp_0.29.0_linux_amd64 /usr/frp
sudo vim /usr/frp/frps.ini  # 编辑frp服务端配置
sudo vim /lib/systemd/system/frps.service  # 注册frp服务端服务
sudo systemctl enable frps  # 开机启动
sudo systemctl start frps  # 启动
sudo systemctl status frps  # 查看运行状态
```

#### frps.ini

```
[common]
bind_port = xxx1
vhost_http_port = xxx2
vhost_https_port = xxx3
token = xxxxxx1
dashboard_port = xxx4
dashboard_user = xxx
dashboard_pwd = xxxxxx2
```

#### frps.service

```
[Unit]
Description=Frp Server Service
After=network.target

[Service]
Type=simple
User=nobody
Restart=on-failure
RestartSec=5s
ExecStart=/usr/frp/frps -c /usr/frp/frps.ini

[Install]
WantedBy=multi-user.target
```

### 树莓派4B

```
wget https://github.com/fatedier/frp/releases/download/v0.29.0/frp_0.29.0_linux_arm.tar.gz
tar -zxvf frp_0.29.0_linux_arm.tar.gz
sudo mv frp_0.29.0_linux_arm /usr/frp
sudo vim /usr/frp/frpc.ini  # 编辑frp客户端配置
sudo vim /lib/systemd/system/frpc.service  # 注册frp客户端服务
sudo systemctl enable frpc  # 开机启动
sudo systemctl start frpc  # 启动
sudo systemctl status frpc  # 查看运行状态
```

#### frpc.ini

````
[common]
server_addr = xxx.xxx.xxx.xxx / domain
server_port = xxx
token = xxxxxx

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = xxx1
remote_port = xxx2

[web]
type = http
local_ip = 127.0.0.1
local_port = 80
custom_domains = xxx.xxx.xxx.xxx / domain
````

#### frpc.service

````
[Unit]
Description=Frp Client Service
After=network.target

[Service]
Type=simple
User=nobody
Restart=on-failure
RestartSec=5s
ExecStart=/usr/frp/frpc -c /usr/frp/frpc.ini

[Install]
WantedBy=multi-user.target
````

## 树莓派功能

### 作为服务器

安装并运行nginx，完成内网穿透，就能够从外部访问树莓派的网站了

### SSH远程访问

完成内网穿透，就通过下面的指令使用SSH远程操作树莓派了

```
ssh -p {remote_port} {user}@{xxx.xxx.xxx.xxx | domain}
```

### NAS

#### 硬盘挂载

我的硬盘格式是exFAT的，所以

```
sudo apt-get install exfat-fuse  # 安装扩展
sudo modprobe fuse  # 加载内核支持
sudo fdisk -l  # 查看存储列表，找到设备，如sda1
sudo mkdir /media/yingpan  # 创建挂载目录
sudo mount /dev/sda1 /media/yingpan  # 挂载
df -h  # 查看挂载列表
```

挂载完毕

##### 设置开机自动挂载

```
sudo blkid  # 查看硬盘UUID
sudo vim /etc/fstab
```
在最后一行添加
```
UUID="" /media/yingpan auto rw,defaults,noexec,umask=0000 0 0
```

#### 文件共享(局域网NAS)

```
sudo apt-get install samba samba-common-bin
sudo vim /etc/samba/smb.conf
```

##### smb.conf
```
[pi]
  path = /media/yingpan/
  valid users = pi#有效用户
  browseable = Yes
  writeable = Yes
  writelist = pi#可写用户列表
  create mask = 0777#新创建文件的默认属性
  directory mask = 0777#新创建文件夹的默认属性
```

```
sudo smbpasswd -a pi  # 添加samba用户
sudo systemctl restart smbd  # 重启服务
sudo systemctl enable smbd  # 开机启动
```

配置完毕即可在局域网访问硬盘上的文件

#### 下载器

```
sudo apt-get install aria2
mkdir -p .config/aria2
touch .config/aria2/aria2.session
vim .config/aria2/aria2.config
```

##### aria2.config

```

#RPC端口, 仅当默认端口被占用时修改
#rpc-listen-port=6800
#最大同时下载数(任务数), 路由建议值: 3
max-concurrent-downloads=5
#断点续传
continue=true
#同服务器连接数
max-connection-per-server=5
#最小文件分片大小, 下载线程数上限取决于能分出多少片, 对于小文件重要
min-split-size=10M
#单文件最大线程数, 路由建议值: 5
split=10
#下载速度限制
max-overall-download-limit=0
#单文件速度限制
max-download-limit=0
#上传速度限制
max-overall-upload-limit=0
#单文件速度限制
max-upload-limit=0
#断开速度过慢的连接
#lowest-speed-limit=0
#验证用，需要1.16.1之后的release版本
#referer=*
#文件保存路径, 默认为当前启动位置(我的是外置设备，请自行坐相应修改)
dir=/media/share
#文件缓存, 使用内置的文件缓存, 如果你不相信Linux内核文件缓存和磁盘内置缓存时使用, 需要1.16及以上版本
#disk-cache=0
#另一种Linux文件缓存方式, 使用前确保您使用的内核支持此选项, 需要1.15及以上版本(?)
#enable-mmap=true
#文件预分配, 能有效降低文件碎片, 提高磁盘性能. 缺点是预分配时间较长
#所需时间 none < falloc ? trunc << prealloc, falloc和trunc需要文件系统和内核支持
file-allocation=prealloc
#不进行证书校验
check-certificate=false
#保存下载会话
save-session=/home/pi/.config/aria2/aria2.session
input-file=/home/pi/.config/aria2/aria2.session
#断电续传
save-session-interval=60
#DMCA
enable-dht=true
bt-enable-lpd=true
enable-peer-exchange=true
#bt-tracker
bt-tracker=udp://62.138.0.158:6969/announce,udp://188.241.58.209:6969/announce,udp://151.80.120.112:2710/announce,udp://151.80.120.112:2710/announce,udp://185.19.107.254:80/announce,udp://93.158.213.92:1337/announce,udp://185.225.17.100:1337/announce,udp://208.83.20.20:6969/announce,udp://5.206.19.247:6969/announce,udp://37.235.174.46:2710/announce,udp://142.44.243.4:1337/announce,udp://195.154.52.99:80/announce,udp://159.100.245.181:6969/announce,udp://89.234.156.205:451/announce,udp://45.56.74.11:6969/announce,udp://54.37.235.149:6969/announce,udp://51.15.226.113:6969/announce,udp://184.105.151.164:6969/announce,udp://176.113.71.19:6961/announce,udp://46.148.18.254:2710/announce
```
(bt-tracker最新地址)[https://github.com/ngosang/trackerslist]

##### 自启动

```
sudo vim /lib/systemd/system/aria2.service
```

```
[Unit]
Description=Aria2 Service
Requires=network.target
After=dhcpcd.service

[Service]
User=pi
Type=forking
ExecStart=/usr/bin/aria2c --conf-path=/home/pi/.config/aria2/aria2.config
ExecReload=/usr/bin/kill -HUP $MAINPID
RestartSec=1min
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

```
sudo systemctl daemon-reload
sudo systemctl enable aria2
sudo systemctl start aria2
```

##### WebUI

(AriaNg)[https://github.com/mayswind/AriaNg]

```
wget https://github.com/mayswind/AriaNg/releases/download/1.1.3/AriaNg-1.1.3.zip
unzip AriaNg-1.1.3.zip -d /var/www/html/aria
```
之前已经配置过nginx，可以直接访问

在 frpc.ini 中新增

```
[aria2-rpc]
type = tcp
local_ip = 127.0.0.1
local_port = 6800
remote_port = 6800
```

### FTP

```
sudo apt install vsftpd

```


## 参考

- (树莓派3B+安装Aria2远程下载服务器)[https://blog.gaozhe.top/essay/85.html]
- (树莓派下Aria2利用frp开启内网穿透)[https://blog.gaozhe.top/essay/97.html]