---
title: React Native 原生组件 -- TextInput
tags:
  - react-native
categories:
  - 移动开发
---

#### 问题总结

##### 页面初始时获取焦点
```javascript
...
componentDidMount(){
  this.refs.input.focus() // iOS 上有效
}
render(){
  return (
    ...
    <TextInput
      ref='input'
      autoFocus={true} // Android 上有效
      />
    ...
  )
}
...
```