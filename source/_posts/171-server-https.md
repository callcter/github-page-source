---
title: 服务器配置 https 访问
---

小程序 request 请求后台必须使用 https，所以我的网站必须升级为 https

#### 配置列表

- ubuntu 16.04
- nginx 1.10.1

#### 证书

阿里云签发的免费证书，期限是一年

#### 配置文件

```
server {
  listen 80;
  listen 443 ssl;
  server_name  115.28.16.103 www.dreamser.com dreamser.com;
  ssl on;
  ssl_certificate cert/dreamser.pem;
  ssl_certificate_key cert/dreamser.key;
  ssl_session_timeout 5m;
  ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_prefer_server_ciphers on;
  root /home/www/station;
  location ~ {
    proxy_set_header   X-Real-IP            $remote_addr;
    proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
    proxy_set_header   Host                   $http_host;
    proxy_set_header   X-NginX-Proxy    true;
    proxy_set_header   Connection "";
    proxy_pass         http://127.0.0.1:4003;
  }
}
```