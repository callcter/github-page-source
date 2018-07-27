---
title: React Native 原生 API -- Animated
tags:
  - react-native
categories:
  - 移动开发
---

获取节点
```javascript
<Animated.View ref={c=>this.view=c}>
</Animated.View>
this.view.getNode()
// 可以使用 measure 等组件方法，如果使用 this.view，会报错 xxx is not a function

```