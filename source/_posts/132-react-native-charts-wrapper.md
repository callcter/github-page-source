---
title: react-native-charts-wrapper 使用说明
tag:
  - react-native
---

### 配置

#### Pod方式

如果只使用 Pods 管理 Charts 跟 SwiftyJSON 两个包 [RN集成react-native-charts-wrapper不完全指南](http://www.kuajieyuan.com/post/H1xQuT5lQm)，如果全部使用 Pods 管理，详见 [react-native-charts-wrapper](https://github.com/wuxudong/react-native-charts-wrapper) 官方配置 IOS

#### 非 Pod 方式

- 推荐这篇文章：[React Native图表插件react-native-charts-wrapper集成教程](https://www.jianshu.com/p/432517c5b531)，文章也有不太对的地方，Bridging-Header 的配置要看组建的 readme，[react-native-charts-wrapper](https://github.com/wuxudong/react-native-charts-wrapper)
- iOS 端配置推荐自己去 github 下载最新版的 charts 跟 swiftJson
- 如果是纯前端的同学，不太推荐使用 pod

### 使用文档

首先推荐看一下 [MPAndroidChart 的 Wiki](https://github.com/PhilJay/MPAndroidChart/wiki) 和 node_modules/react-native-charts-wrapper/lib 下的源码，然后才是我的简单说明

#### 基本配置

源码位置：ChartBase.js
```jsx
<Chart
  legend={} // 图例配置，详细的在后面
  xAxis={} // X轴配置
  yAxis={} // Y轴配置
  data={} // 图表的数据
  chartDescription={} // 图表的描述，{text: 'xxx'}，如果没有该项，Android会有默认的水印，不用的话要赋 '' 值，源码中 descriptionIface
  viewPortOffsets={} // 边距，{top: 0, left: 0, right: 0, bottom: 0}
  />
```

#### Legend 图例

```jsx
{
  enabled: PropTypes.bool, // 是否使用图例
  textColor: PropTypes.number, // 文字颜色
  textSize: PropTypes.number, // 文字大小
  fontFamily: PropTypes.string, // 字体
  fontStyle: PropTypes.number, // 文字样式
  fontWeight: PropTypes.number, // 文字粗细
  wordWrapEnabled: PropTypes.bool, // 文字换行
  maxSizePercent: PropTypes.number, // 最大占用位置，1 为全占
  horizontalAlignment: PropTypes.string, // 水平位置：LEFT, CENTER, RIGHT
  verticalAlignment: PropTypes.string, // 垂直位置：TOP, CENTER, BOTTOM
  orientation: PropTypes.string, // HORIZONTAL, VERTICAL
  drawInside: PropTypes.bool,
  direction: PropTypes.string, // LEFT_TO_RIGHT, RIGHT_TO_LEFT
  form: PropTypes.string, // 图例图标类别：NONE, EMPTY, DEFAULT, SQUARE, CIRCLE, LINE
  formSize: PropTypes.number, // 图例图标大小
  xEntrySpace: PropTypes.number, // 横向间隙
  yEntrySpace: PropTypes.number, // 竖向间隙
  formToTextSpace: PropTypes.number, // 图标与文字的间隙
  custom: PropTypes.shape({
    colors: PropTypes.arrayOf(PropTypes.number), // 自定义颜色
    labels: PropTypes.arrayOf(PropTypes.string) // 自定义名字
  })
}
```

#### xAxis X轴

#### yAxis Y轴

#### data 数据

```jsx
// 单类
{
  dataSets: [{}]
}
// 混合 CombinedChart
{
  lineData: {
    dataSets: [{
      label: '' // 名称
      values: [] // y 轴的值，格式：[val1, val2, ...] 或 [{y: val1}, {y: val2}, ...]，对象数组不能有x，或其他
      config: {} // 配置
    }]
  },
  barData: {
    dataSets: []
  }
}
```

### 兼容问题

- 处理 null，Android 不允许配置项为 null，许多闪退问题都是因为这个
- 使用 barChart group 配置必须保证至少有两个 bar，也会闪退