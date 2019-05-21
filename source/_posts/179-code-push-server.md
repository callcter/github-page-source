---
title: 搭建 CodePush 服务器
---

官方的README很好，完全可以按照那个来，只是有几个地方

1. 使用 pm2 管理进程始终有问题，我自己换成了 forever
2. 我使用 export NODE_ENV=production 的方式将环境变量写入 profile 文件，没有选择在进程中配置

### 注意

- 使用自己搭建的服务器在集成 react-native-code-push 时要记得修改 info.plist 和 MainApplication.java，添加服务器的地址，详细方式可见 [code-push-server 使用+一些需要注意的地方](https://github.com/lisong/code-push-server/issues/135)——react-native项目配置和发布

### 参考文章

- [github repo](https://github.com/lisong/code-push-server)
- [code-push-server 使用+一些需要注意的地方](https://github.com/lisong/code-push-server/issues/135)