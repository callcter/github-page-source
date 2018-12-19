---
title: Mac 上使用 Charles 进行手机抓包调试
---

RN 开发时虽然可以借助 remote-devtools 等工具在 chrome 中调试，但是在有些场景下，比如微信登录，不能用 chrome 调试，这个时候就需要抓包工具的帮助了，在 Mac 上首选 Charles。

<!-- more -->

#### 安装

- 官网下载地址 [https://www.charlesproxy.com/download/latest-release/](https://www.charlesproxy.com/download/latest-release/)
- 注册方式（测试有效）
Registered Name: https://zhile.io
License Key: 48891cf209c6d32bf4

#### 配置

1. 配置代理端口号，我用的是 8888
![](http://cdn.dreamser.com/mac-charles-android-1.png)
![](http://cdn.dreamser.com/mac-charles-android-2.png)

2. 配置手机代理，主机名是 mac 的 IP 地址，可以使用 ifconfig 指令查看
![](http://cdn.dreamser.com/mac-charles-android-3.png)

这样就可以正常抓包 http 请求了

#### https

1. 配置 ssl proxy，我直接使用的是 *:*，也可以有针对的配置
![](http://cdn.dreamser.com/mac-charles-android-4.png)
![](http://cdn.dreamser.com/mac-charles-android-5.png)
![](http://cdn.dreamser.com/mac-charles-android-6.png)

2. Mac 上安装证书
![](http://cdn.dreamser.com/mac-charles-android-7.png)
点击后会自动打开 Mac 的钥匙串管理，找到刚刚添加的 Charles 的证书
![](http://cdn.dreamser.com/mac-charles-android-8.png)
设置为”始终信任“
![](http://cdn.dreamser.com/mac-charles-android-9.png)

3. 手机上添加证书
![](http://cdn.dreamser.com/mac-charles-android-10.png)
![](http://cdn.dreamser.com/mac-charles-android-11.png)
使用手机浏览器打开上面的链接，会下载证书文件，可能是 .pem 文件，也可能是 .crt 文件，直接打开，如果不能直接打开，可以通过从设备中找到证书文件安装，效果如下
![](http://cdn.dreamser.com/mac-charles-android-12.png)
安装时需要手机密码，安装后可以在信任列表中找到刚刚安装的证书
![](http://cdn.dreamser.com/mac-charles-android-13.png)

#### 问题

1. 小米浏览器下载显示无法打开文件

在 设置 -> 更多设置 -> 系统安全 -> 加密与凭据 中，选择 "从存储设备安装"，然后找到下载的证书文件安装

可能因为系统版本不同略有差异，但大抵相同


2. 遵照配置安装证书之后 https 请求显示的还是 unknown

原因：这是 Android 7.0 及之后的系统版本的安全策略，APP 需要添加网络安全性配置才能进行调试
解决方法：在 APP 内添加安全配置文件

- 修改 AndroidManifest.xml 文件
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <application android:networkSecurityConfig="@xml/network_security_config"
        ... >
        ...
    </application>
</manifest>
```
- 在 res 目录下创建 xml 目录，在目录下创建 network_security_config.xml 文件（文件名一致即可）
- network_security_config.xml 文件内容如下
```xml
<!-- 限制可信 CA 集，res 目录下创建 raw 目录，将证书文件（pem或der格式）放到目录下，假如证书文件名是 trusted_roots，然后明确信任的域名 -->
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <domain-config>
        <domain includeSubdomains="true">secure.example.com</domain>
        <domain includeSubdomains="true">cdn.example.com</domain>
        <trust-anchors>
            <certificates src="@raw/trusted_roots"/>
        </trust-anchors>
    </domain-config>
</network-security-config>

<!-- 信任一切证书 -->
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config cleartextTrafficPermitted="true">
        <trust-anchors>
            <certificates src="system" overridePins="true" /> <!--信任所有用户-->
            <certificates src="user" overridePins="true" /> <!--信任某位用户-->
        </trust-anchors>
    </base-config>
</network-security-config>
```

3. Android Studio 同步时报错：Gradle 'app' project refresh failed: Unable to find valid certification path to requested target

原因：Charles 会默认打开系统代理，所以 Android Studio 在 sync project 的时候，会经过 Charles 的代理，又因为 Charles 设置了根证书，所以 AS 在 sync project 的时候就报了‘找不到证书’的错误

解决方法：Charles -> Proxy -> Proxy Settings -> macOS
在 macOS 里把 Enable macOS proxy 和 Enable macOS proxy on launch 的勾去掉，然后重新启动一下 Charles ，重启 AS 就可以正常编译了


#### 参考文章

- [解决Charles https抓包显示](https://blog.csdn.net/Zachary_46/article/details/81458098)
- [Android7.0、8.0、9.0的https抓包，charles解决方案](https://blog.csdn.net/cadi2011/article/details/83056527)
- [网络安全性配置](https://developer.android.google.cn/training/articles/security-config#manifest)