---
title: 在 React Native 中使用 CocoaPod
tag:
  - react-native
---

#### 卸载旧版

请见参考一

#### 安装

```
sudo gem install cocoapods
pod setup // 运行很慢，因为要从github下载很多文件，优化速度请看 [请看 github 加速](http://liu-hang.cn/2018/08/27/github-speed-up/)
```

#### 使用

##### 创建 Podfile 文件
```
pod init
```

##### 编辑 Podfile
根据使用的组件，编辑 Podfile，如 [react-native-image-crop-picker](https://github.com/ivpusic/react-native-image-crop-picker) 和 [react-native-charts-wrapper](https://github.com/wuxudong/react-native-charts-wrapper/blob/master/installation_guide/README.md)

##### 安装插件
```
pod install
```
##### 重新打开
关闭 .xcodeproj，打开 .xcworkspace

#### 参考

1. [安装cocoapods以及正确在react-native项目中应用](https://blog.csdn.net/qq_37336604/article/details/80340968)