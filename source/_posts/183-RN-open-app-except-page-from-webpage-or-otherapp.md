---
title: React Native 实现从浏览器或其他APP唤醒APP并跳转指定页面
---

### APP环境
- react 0.59.5
- react-navigation 3.9.1
- react-navigation-redux-helpers 3.0.2

### React Native 实现
我使用 react-navigation 来管理路由，react-navigation本身支持 [Deep Linking](https://reactnavigation.org/docs/en/deep-linking.html)，但是我使用 react-navigation-redux-helpers 组件将路由也封装在 redux 下了，所以 react-navigation 本身的 deep-linking 功能就不能用了，在 react-navigation-redux-helpers 的 issue 下面我找到了其他的解决方案。

```
// 为了监听APP从后台唤起的行为，所以我选择使用 AppState 接口，然后使用 Linking 获取 url，然后将获取到的地址初步解析提交给 LinkRoutes 方法
import { Linking, AppState } from 'react-native'
import LinkRoutes from './src/routes/linkRoutes'
...
componentDidMount(){
  AppState.addEventListener('change', this._handleAppStateChange)
}
componentWillUnmount(){
  AppState.removeEventListener('change', this._handleAppStateChange)
}
_handleAppStateChange = (nextAppState) => {
  if(nextAppState==='active'){
    Linking.getInitialURL().then(res => {
      if(!!res){
        LinkRoutes(res.split(':/')[1])
      }
    })
  }
}
...
```

```
// LinkRoutes 的实现，使用了 path-parser 做 url 解析，然后维护 APP 内需要跳转的页面的列表，然后调用 react-navigation 的navigate 方法，使用 dispatch 实现跳转到指定页面
import PathParser from 'path-parser'
import { NavigationActions } from 'react-navigation'
import store from '../reducers'
import _ from 'lodash'

const paths = [
  {
    routeName: 'Page',
    path: new PathParser('/page')
  }
]

const findPath = url => {
  let idx = _.findIndex(paths, p => {
    return p.path.test(url)
  })
  return idx > -1 ? paths[idx] : false
}

export default url => {
  const pathObj = findPath(url)
  if(!pathObj) return
  const navigateAction = NavigationActions.navigate({
    routeName: pathObj.routeName,
    params: pathObj.path.test(url)
  })
  store.dispatch(navigateAction)
}
```

### iOS配置
```
// 修改 AppDelagate.m，添加
#if __IPHONE_OS_VERSION_MAX_ALLOWED > 100000
// 最新版本系统回调
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey, id> *)options
{
  BOOL result = [[UMSocialManager defaultManager]  handleOpenURL:url options:options];
  if (!result) {
    // 其他如支付等SDK的回调
  }
  return result;
}
#endif

// 支持所有iOS系统，此方法在swift4.1(Xcode 9.3)已废弃，Objective-C项目不影响。
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation
{
  BOOL result = [[UMSocialManager defaultManager] handleOpenURL:url sourceApplication:sourceApplication annotation:annotation];
  if (!result) {
    // 其他如支付等SDK的回调
  }
  return result;
}

- (BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url
{
  BOOL result = [[UMSocialManager defaultManager] handleOpenURL:url];
  if (!result) {
    // 其他如支付等SDK的回调
  }
  return result;
}
```

然后，修改 Url Types，添加 myapp

测试指令
```
xcrun simctl openurl booted myapp://
```

iOS 使用模拟器是 Linking 捕捉不到 url，所以我是打包后在手机上测试的

### Android配置

在 .MainActivity 下添加配置，还可以设置host以及path等，但是因为我是通过RN做解析，这里就不加了

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data android:scheme="myapp" />
</intent-filter>
```

测试指令
```shell
adb shell am start -W -a android.intent.action.VIEW -d "myapp://" com.myapp
```

### 参考文章

- [React Navigation Deep linking](https://reactnavigation.org/docs/en/deep-linking.html)
- [Redux can not be used with deep linking](https://github.com/react-navigation/react-navigation/issues/1189)
- [react native 实现浏览器唤醒APP并跳转指定页面](https://blog.csdn.net/u010379595/article/details/83622335)