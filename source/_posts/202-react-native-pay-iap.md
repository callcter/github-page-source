---
title: React Native 支付集成（iOS内购）
---

到目前为止，我们的内购还没上线，先把坑记下来，请按步骤进行。文章没有截图等详细的步骤，仅仅是一个流程，然后附带一些可以参考的文章，哪怕是一个新手，也可以按照这个流程完成内购的开发，我就是个新手...

<!-- more -->

#### 坑们

- 创建新应用内购买项目需要提交新的构建版本，但是不用提交审核，不用提交审核，不用提交审核，即可沙箱测试
- 每个商品ID都是唯一的，删掉了之后也不能用了

#### 1. 银行协议填写

想要开通应用付费或者添加应用内购买项目，首先要完善付费协议，这是一篇非常详细的介绍 [《2019年苹果开发者申请IOS内购银行协议填写教程》](http://bbs.yimenapp.com/forum.php?mod=viewthread&tid=11531)，一步步走下来没有问题。

#### 2. 开通应用内购买

- 创建的 APP ID 需要勾选 “In-App Purchase”
- Xcode 中开启 “In-App Purchase” 功能

#### 3. 创建应用内购买项目

操作步骤这篇文章[《iOS 内购（In-App Purchase）总结》](https://xiaovv.me/2018/05/03/My-iOS-In-App-Purchase-Summarize/)说得很明了，只需要看文章的添加应用内购买项目的部分就可以了。

#### 4. 内购功能开发

我选择的组件是 [react-native-iap](https://github.com/dooboolab/react-native-iap)，请仔细阅读 readme。

基本内容如下：

```js
initConnection  // 初始化连接
purchaseUpdatedListener & purchaseErrorListener  // 购买监听
getProducts ｜ getSubscriptions  // 商品或订阅列表获取
requestPurchase ｜ requestSubscription  // 购买商品或订阅
finishTransactionIOS  // 确认订单
```

#### 5. 沙箱测试

###### 5.1 创建沙箱测试账号

如[《iOS 内购（In-App Purchase）总结》](https://xiaovv.me/2018/05/03/My-iOS-In-App-Purchase-Summarize/)的“配置沙箱测试帐号”

###### 5.2 提交新的构建版本

添加新的应用内购买项目时，必须要提交新的构建版本，但是不需要提交审核，在 “**等待提交**” 状态下，将新的应用内购买项目添加到版本中，即可进行沙箱测试

###### 5.3 进行沙箱测试

参见[《【iOS】苹果IAP(内购)中沙盒账号使用注意事项》](https://www.jianshu.com/p/1ef61a785508)

#### 6. 应用上架

可能会被退回哟，按反馈改进就行了...

#### 参考文章

- [2019年苹果开发者申请IOS内购银行协议填写教程](http://bbs.yimenapp.com/forum.php?mod=viewthread&tid=11531)
- [iOS 内购（In-App Purchase）总结](https://xiaovv.me/2018/05/03/My-iOS-In-App-Purchase-Summarize/)
- [【iOS】苹果IAP(内购)中沙盒账号使用注意事项](https://www.jianshu.com/p/1ef61a785508)