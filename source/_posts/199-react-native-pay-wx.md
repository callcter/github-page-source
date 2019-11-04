---
title: React Native 支付集成（微信）
---

我仅仅做了 Android 的集成，iOS使用的是应用内购买，后面会有文章介绍

<!-- more -->

#### 坑们

- 微信的文档屎一样
- 微信官方SDK 可能跟 Umeng 的有冲突，如果已经有 Umeng 的集成，请选择精简版的Umeng微信SDK，然后自己添加微信的SDK
- 微信的沙箱测试问题很多，可以把金额改为 0.01 之类很小的数值直接产品测试

#### 开发

请按照参考文章来，很完整，唯一缺少的地方是 AndroidManifest.xml 的配置

```xml
<activity
  android:name=".wxapi.WXPayEntryActivity"
  android:exported="true" />
```

华为手机居然不需要添加微信支付的Activity就可以用？？？

#### 参考文章

- [react-native集成微信支付](https://www.jianshu.com/p/a414cad81c9a)