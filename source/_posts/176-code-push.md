---
title: react-native-code-push 集成
---

#### 流程

- 安装 CodePush CLI
- 注册 CodePush账号
- 在CodePush服务器注册App
- RN代码中集成CodePush
- 原生应用中配置CodePush
- 发布更新的版本

#### 安装

```
npm i -g code-push-cli
```

#### 注册

```
code-push register
```
其他指令
```
code-push login // 登陆
code-push loout // 注销
code-push access-key ls // 列出登陆的token
code-push access-key rm <accessKye> // 删除某个 access-key
```

#### 添加 APP

```
code-push app add ProjectName <ios|android> react-native
```
其他指令
```
code-push app add // 在账号里面添加一个新的app
code-push app remove // 或者 rm 在账号里移除一个app
code-push app rename // 重命名一个存在app
code-push app list // 或则 ls 列出账号下面的所有app
code-push app transfer // 把app的所有权转移到另外一个账号
```

#### RN集成

在项目中安装 react-native-code-push
```
npm i react-native-code-push
react-native link react-native-code-push
```
代码 index.js
```
import { AppRegistry } from 'react-native'
import CodePush from 'react-native-code-push'
import App from './App'
import {name as appName} from './app.json'

const codePushOptions = {
  checkFreQuency: CodePush.CheckFrequency.MANUAL
}

const AppWithCodePush = CodePush(codePushOptions)(App)

AppRegistry.registerComponent(appName, () => AppWithCodePush)
```
代码 App.js
```
import CodePush from 'react-native-code-push'
export default class App extends React.Component {
  componentWillMount(){
    CodePush.disallowRestart()
    CodePush.sync({
      updateDialog: false,
      installMode: CodePush.InstallMode.IMMEDIATE
    })
  }
  componentDidMount(){
    CodePush.allowRestart()
  }
}
```

#### 参看文章
- [CodePush热更新详细接入教程](https://www.jianshu.com/p/6a5e00d22723)
- [react-native-code-push进阶篇](https://www.jianshu.com/p/6e96c6038d80)
- [App Center](https://appcenter.ms)
- [Microsoft/react-native-code-push](https://github.com/Microsoft/react-native-code-push)