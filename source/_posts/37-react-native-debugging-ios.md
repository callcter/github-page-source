---
title: React Native调试—iOS
tags:
  - react-native
url: 131.html
id: 131
categories:
  - 移动开发
date: 2016-07-20 14:34:04
---

#### 开发环境搭建

详细请看官方文档：[https://reactnative.cn/docs/getting-started.html](https://reactnative.cn/docs/getting-started.html)

#### 项目运行

##### 基本开发方式

使用Xcode打开目录中的 xxx.xcodeproj 项目文件，Command+R 或 点击运行按钮 运行项目，项目就会在模拟器中打开

##### 使用CocoaPods

在使用头像截取&图片多选插件 react-native-iamge-crop-picker 时遇到这种情况，使用方法可以查看使用指南 [https://github.com/ivpusic/react-native-image-crop-picker](https://github.com/ivpusic/react-native-image-crop-picker)

#### 调试

##### 模拟器

使用Chrome，在模拟器下，Command+D弹出菜单，选择 “Remote JS Debug”，会自动弹出浏览器，通过开发者工具，可以查看调试

##### 真机

因为APP需要不停的需改，推荐在模拟器开发完成并且做好基本测试之后再到真机上运行，而且相比Android，iOS的模拟器做的太贴心了

#### 实时刷新

使用LiveReload实现实时刷新，首先在Chrome中安装LiveReload插件，然后在模拟器菜单中选择“Enable Live Reload”，就能够实现代码修改保存，模拟器自动刷新了 真机貌似不行

#### 注意

##### 功能兼容

如 image-crop-picker 插件用到 PhotoFrame，这个组件在ios7中不支持，但是只有iPhone4不能够升级系统，而且iPhone4的市场占有率目前已不足2%，可以考虑放弃

##### 插件使用

某些插件使用时需要引入库文件，建立静态链接，详细步骤可见：[http://www.lcode.org/](http://www.lcode.org/react-native%E5%9F%BA%E7%A1%80%E4%B9%8Blinking-libraries%E9%93%BE%E6%8E%A5%E5%BA%93%E9%85%8D%E7%BD%AE-%E9%80%82%E9%85%8Dios%E5%BC%80%E5%8F%9149/) 博客作者有很多不错的教程