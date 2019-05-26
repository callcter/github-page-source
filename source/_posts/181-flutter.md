---
title: flutter 入门（Mac）
---

### 背景

近一年时间都在用 React Native 开发公司的 APP，水平基本上可以说是登堂入室了。前一段时间兴起一阵 Flutter 热，后端的同事都有推荐，今年 Google 大会又推出 flutter_web，偶有闲暇，就赶紧来看看这个新的多端兼容框架。

与 RN 的封装原生组件通过 Bridge 调用不同，Flutter 的模式是使用 Google 的 UI 引擎另起炉灶，使用 Dart 语言编写，最终还是编译成原生的代码。性能方面相较RN有所提升，但是RN已经有近5年的发展了，社区相当活跃，Flutter的底层组件完全依赖Google的开发，而且起步晚，所以功能上面并不完备。

安装、配置和入门实例可以按照官方的文档——[Flutter 入门](https://flutterchina.club/get-started/)，文档非常详细。

### 有几个注意的地方

1. Flutter 安装的版本，我选择的是最新版本的 Beta 版本，内容比较新而且问题相对较少
2. 环境变量的配置，我的是 Mac + zshell，所以我将变量写进 .zshrc 文件中
3. 编辑工具的选择，我暂时用的是 Android Studio
4. Android Studio 安装 Flutter 和 Dart 插件后在启动预览界面会出现“Start a new Flutter project”的选项，点击之后需要等待一段时间，并不是进程死掉
5. 新建项目时，配置完成之后开始创建，弹出“Creating Flutter Project”的窗口，会一直卡住，我开VPN用全局模式都不行，大概10分钟左右项目基本结构已经创建好了，这时候强制退出 Android Studio 的进程，然后重新打开，选择项目打开，进入项目之后会提示修复项目，马上就能修复好
6. 官方组件地址 [http://pub.dartlang.org/flutter](http://pub.dartlang.org/flutter) 打不开，当然，可能是我们家的网有问题
7. Android 模拟器无法安装实例，我只能用 iOS 的模拟器测试

### 总结

1. 可能是习惯问题，感觉 RN 写起来更舒服一些，用 Wedget 写起来太乱了，而且很死板
2. 相关配套的服务不太完善，如pub等
3. 之前一直在用文本编辑器写代码，用了一下 IDE，好省事