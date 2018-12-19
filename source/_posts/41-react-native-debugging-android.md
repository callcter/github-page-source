---
title: React Native调试—Android
tags:
  - react-native
url: 146.html
id: 146
categories:
  - 移动开发
date: 2016-08-09 17:44:03
---

前面有一篇iOS的调试指南，这次讲解的是Android的调试。 1、Android开发环境搭建 请查看官方文档的开发环境搭建，选择自己的开发平台：[http://reactnative.cn/docs/0.31/getting-started.html#content](http://reactnative.cn/docs/0.31/getting-started.html#content) 2、测试设备 （1）模拟器：

android avd

指令打开模拟器管理器，然后新建打开模拟器 （2）真机： USB连接，然后打开开发者选项，开启USB调试 3、Log查看 （1）Stetho 官方推荐的调试工具，但是好像console不能使用 （2）使用终端

react-native log-android

指令能将全部log打印出来，自我感觉比stetho好用一些，但是没有在真机中测试过 4、注意的几点： （1）如果没有Android的开发经验，环境搭建一定要按照文档的操作来，尤其是 ANDROID_HOME 和 tools、platform-tools “环境变量”的设置，我在这里吃了很多亏，白白耗掉一天时间 （2）新建虚拟机一定要注意勾选 Use Host GPU （3）如果“小米”的真机作为调试设备，在开发者选项中一定要关闭 “MIUI优化”，其他如魅族、华为因为系统都是重置版的原因，多多少都会有一些问题 5、Bug集合 （1）Application ProjectName has not been registered. 解决方案：[http://www.jianshu.com/p/82a09063e61c](http://www.jianshu.com/p/82a09063e61c) （2）:app:installDebug Failed to establish session 解决方案：这就是我在小米上遇到的问题，app-debug.apk安装不了，原因就是因为开启了 ”MIUI优化“ （3）adb not found 解决方案：安装Android Studio的时候这些环境变量都需要自己配置，按照文档将ANDROID_HOME和Tools的环境变量加入终端工具的 rc 文件中就好，我的是.zshrc，你可能使用.bash之类的   Bug和注意事项遇到之后就会更新，这次用Android调试学到了一件很重要的事情，一定要先看官方文档