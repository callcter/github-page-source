---
title: react-native 图表解决方案
tag:
  - react-native
---

### 背景
图表是数据展示的最佳方式，直观，全面

### 方案

#### echarts + WebView

echart 是我用过的图表中最好用的一个，没有之一。虽然 highchart、阿里等都有不错的产品，但是在图表种类、显示效果、社区支持上，echart都是占优的，这也是百度少有的靠谱的产品，现在已经移交到Apache下，希望在全球开发者的努力下能越做越好吧。

##### 优点

1. 完全支持 echart 的配置方式，因为之前在Web端用的就是echart，所以移植起来很方便
2. 配置简单，因为使用的是 WebView，不需要其他的原生组件支持
3. 扩展容易，github 上有 native-echarts 的项目，但是功能不太完备，依赖版本低，所以我重写了一个，只是重新封装了一下，很方便

##### 缺点

1. 性能问题，毫无办法，这是 WebView 的性能局限
2. 部分功能失效，比如 formatter，无法使用回调函数的方式，canvas渲染模式在android失效等等

##### 总结

如果是纯前端开发的同学，又使用过echart，且显示图表交互简单，可以选择此方案

#### react-native-charts-wrapper

github上比较流行的一个图表组件，是对 ios charts 跟 android MPAndroidChart 的封装

##### 优点

1. 因为是原生的封装，所以性能上面肯定是有优势的

##### 缺点

1. 配置麻烦，在iOS中需要额外义勇 swiftJson 跟 charts 两个原生组件，还要建 header file等等，做前端的千万别用 pod，真是费劲
2. 图表功能不如echart多，可配置项很有局限性
3. 文档！文档！只能看 MPAndroidChart 的文档，而且还不一定对应，需要看源码
4. 对前端开发来说太不友好，有错不知道哪里改，痛苦
5. 兼容问题，尤其是 Android

##### 总结

虽然他缺点这么多，但是我们还是选择这个方案，因为性能，因为我们的APP图表上的交互太多了(T_T，真的是把RN看得太高了)，现在正一点儿点儿啃，加油吧，推荐一篇比较好的配置说明——[React Native图表插件react-native-charts-wrapper集成教程](https://www.jianshu.com/p/432517c5b531)，之后会在写一篇图表配置的文章，详细讲解一下图表里的配置项