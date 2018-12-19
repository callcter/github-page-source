---
title: react-navigation TabNavigator 切换时重新拉取数据并渲染
tags:
  - react
  - react-native
---

#### 场景
今天遇到这样一个需求，需要在切换到某个 tab 时重新拉一下数据，但是在原本的使用过程中，切换 tab 时，tab是不会重新渲染的，也不会有拉取数据的机会

#### 解决方案
在 Google 之后发现了 tabBarOnPress 这个方法，并且又加深了对 redux dispatch 的认识
##### 修改 navigationOptions
```jsx
// 初始的代码
{
  // ...
  static navigationOptions = {
    header: null,
    title: '自选',
    tabBarIcon: ({focused, tintColor}) => {
      const iconName = focused ? 'zixuangu' : 'zixuangu'
      return <Iconfont name={iconName} size={24} color={tintColor} />
    }
  }
  // ...
}
// 修改之后
{
  // ...
  static navigationOptions = ({navigation}) => ({
    header: null,
    title: '自选',
    tabBarIcon: ({focused, tintColor}) => {
      const iconName = focused ? 'zixuangu' : 'zixuangu'
      return <Iconfont name={iconName} size={24} color={tintColor} />
    },
    tabBarOnPress: (obj) => {
      // 在 navigationOptions 中拿不到 props，所以使用 dispatch 的方式来调用 action
      navigation.dispatch(loadData())
      // 添加 tabBarOnPress 方法之后需要自己写跳转的代码
      obj.jumpToIndex(obj.scene.index)
    }
  })
  // ...
}
```
##### action 引用方式修改
```jsx
// 修改前
import * as ActionCreators from './action'
// ...
const mapDispatchToProps = (dispatch) => {
  return bindActionCreators({...ActionCreators}, dispatch)
}
// 修改之后，因为直接使用 dispatch 方式调用，所以不需要再用 bindActionCreators 方法
import { loadData } from './action'
```

#### 参考文章
- [react-navigation中TabNavigator切换时重新渲染](https://blog.csdn.net/ky1in93/article/details/80669199)