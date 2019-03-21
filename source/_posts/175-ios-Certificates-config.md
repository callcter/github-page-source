---
title: iOS开发Certificates, Identifiers & Profiles介绍
---

在 iOS 开发过程中，必定需要接触开发者中心的各种证书配置。

<!-- more -->

下图是开发者中心证书配置页面的导航栏，分别是 Certificates、Keys、Identifiers、Devices、Provisioning Profiles
![](https://cdn.dreamser.com/blog/WX20190318-183554.png)

#### Certificates

Certificates 是用来证明iOS App内容（executable code）的合法性和完整性的数字证书。对于想安装到真机或发布到AppStore的应用程序（App），只有经过签名验证（Signature Validated）才能确保来源可信，并且保证App内容是完整、未经篡改的。

> 数字证书是一个经证书授权中心数字签名的包含公开密钥拥有者信息以及公开密钥的文件。具有时效性，只在特定的时间段内有效。

iOS证书分为”开发证书“和”生产证书“两种，开发证书用于开发和调试APP，生产证书用于发布APP。然后证书根据具体用途，还有其他分类，如消息推送证书、支付证书等。下图是全部种类的证书，在开发时根据需求生成。

![](https://cdn.dreamser.com/blog/WX20190321-194802.png)

#### Keys

Keys 是APP开启服务的凭证，与证书的作用类似，但是可以开启的服务较少。

![](https://cdn.dreamser.com/blog/WX20190321-200500.png)

#### Identifiers

Identifiers 即标识符，相当于应用或服务的身份证，最常用的就是我们开发APP时使用的App IDs。

App ID分为Explicit（精确）和Wildcard（通配）两种，Explicit类型的App ID需要跟APP的bundle id一样。同时，在创建App ID时需要选择包含的服务，如推送、支付等。下图是App ID的创建页面。

![](https://cdn.dreamser.com/blog/WX20190321-202004.png)

#### Devices

Devices 是指运行iOS系统用于开发调试App的设备（即苹果设备）。每台Apple设备使用UDID来唯一标识。在创建Provisioning Profiles时需要选择设备，在证书中包含的设备才能安装APP。

#### Provisioning Profiles

Provisioning Profiles 是应用的配置文件，其中包含了 证书、App ID和设备，也是根据用途有不同的种类。

![](https://cdn.dreamser.com/blog/WX20190321-202953.png)

最后在Xcode的配置

![](https://cdn.dreamser.com/blog/WX20190321-203424.png)


#### 参考文章

- [iOS开发证书与配置文件的使用](https://www.jianshu.com/p/9d9e3699515e)
- [iOS 开发者账号和证书](https://www.jianshu.com/p/b3ff7cf0f605)