---
title: 更合理的 setState()
tags:
  - react
  - react-native
---

新看到一种 setState 的使用方式，之前只是使用第一种最简单的方式，看到这篇文章，长了姿势

```js
// 方式一
this.setState({
  count: this.state.count + 1
})
// 方式二
this.setState(() => ({
  count: this.prevState.count + 1
}))
```

##### 更新

新遇到了一种使用场景，在实现列表全选时，如果使用方式一，state 只能保存列表中一项的状态，修改为方式二时，就可以序列化改变 state，最终保留所有项

[更合理的 setState()](https://www.cnblogs.com/libin-1/p/6725774.html)