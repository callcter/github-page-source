---
title: React Native项目打包-iOS
tags:
  - react-native
url: 548.html
id: 548
categories:
  - 移动开发
date: 2018-04-26 08:57:31
---

### 打包静态文件

开发React Native的过程成,js代码和图片资源运行在一个Debug Server上，每次更新代码之后只需要使用command+R键刷新就可以看到代码的更改，这种方式对于调试来说是非常方便的。 但当我们需要发布App到App Store的时候就需要打包,使用离线的js代码和图片。这就需要把JavaScript和图片等资源打包成离线资源，在添加到Xcode中，然后一起发布到App Strore中。 打包离线资源需要使用命令react-native bundle 我们打包所用到的命令：

    react-native bundle --platform ios --dev false --entry-file index.js --bundle-output ./ios/bundle/index.ios.jsbundle --assets-dest ./ios/bundle
    

查看 ios/bundle 目录，有 index.ios.jsbundle说明打包成功 - --entry-file ,ios或者android入口的js名称，比如index.ios.js

*   --platform ,平台名称(ios或者android)
    
*   --dev ,设置为false的时候将会对JavaScript代码进行优化处理。
    
*   --bundle-output, 生成的jsbundle文件的名称，比如./ios/bundle/index.ios.jsbundle
    
*   --assets-dest 图片以及其他资源存放的目录,比如./ios/bundle
    

命令的详细介绍：[https://github.com/facebook/react-native/blob/master/local-cli/bundle/bundleCommandLineArgs.js](https://github.com/facebook/react-native/blob/master/local-cli/bundle/bundleCommandLineArgs.js) 为了方便使用，可以将命令写入 package.json

    npm run bundle-ios
    

通过命令执行 ps. 命令可能报错：ENOENT: no such file or directory, open './ios/bundle/index.ios.jsbundle' 原因是打包前需要在 ios 目录下创建 bundle 目录

### 引入静态文件

*   Add Files to "app"

![](http://cdn.dreamser.com/wp-content/uploads/2018/04/屏幕快照-2018-04-26-上午8.28.34.png)

*   选择 bundle 文件，选择 Create folder reference，勾选 app，点击 Add 添加

![](http://cdn.dreamser.com/wp-content/uploads/2018/04/屏幕快照-2018-04-26-上午8.32.17.png)

*   添加到项目下的文件必须是蓝色的

![](http://cdn.dreamser.com/wp-content/uploads/2018/04/屏幕快照-2018-04-26-上午8.36.03.png)

### jsCodeLocation

修改 AppDelegate.m 文件，将 debug 的jsCodeLocation 修改为 release 的

    // for debug
    // jsCodeLocation = [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index" fallbackResource:nil];
    // for release
    jsCodeLocation = [[NSBundle mainBundle] URLForResource:@"bundle/index.ios" withExtension:@"jsbundle"];
    

### End

经过上述步骤，再使用 xcode 运行项目的时候，App 内所调用的文件就是离线的了，n然后可以通过正常的 APP 发布流程，导出，发布到 App Store