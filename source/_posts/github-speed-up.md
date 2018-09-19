---
title: github访问加速
tag:
  - git
---

#### 获取 IP

在 [https://www.ipaddress.com/](https://www.ipaddress.com/) 使用 IP LookUp 工具获得 github.com 与 github.global.ssl.fastly.net 的 ip，比如我获得的分别是 192.30.253.112 和 151.101.185.194

#### 修改 hosts

在 hosts 文件中添加这两条
```
192.30.253.112 github.com
151.101.185.194 github.global.ssl.fastly.net
```

#### 清空 DNS 缓存

执行如下命令
```
sudo dscacheutil -flushcache
```

然后使用 github 下载项目就快多了，再也不用苦苦等待 npm install、pod install、git clone 等等操作的卡槽了