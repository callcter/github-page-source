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
code-push logout // 注销
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

#### 发布更新

```
code-push release-react <appName> <platform> --t <version> --d <deploymentName>
```

#### 回滚
```
code-push rollback <appName> <deploymentName>
// code-push rollback MyApp Production --targetRelease v1
```

#### 查看发布历史
```
code-push deployment history <appName> <deploymentName>
```

#### 清除发布历史
```
// 运行此命令后，那些已经配置了使用关联的部署密钥的客户端设备将不再接收被清除掉的更新。这个命令是不可逆的,因此不应该使用在生产部署。
code-push deployment clear <appName> <deploymentName>
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
- [CodePush 命令行中文版](https://github.com/microsoft/code-push/blob/master/cli/README-cn.md)
- [React Native应用部署/热更新-CodePush最新集成总结(新)](https://github.com/crazycodeboy/RNStudyNotes/tree/master/React%20Native%E5%BA%94%E7%94%A8%E9%83%A8%E7%BD%B2%E3%80%81%E7%83%AD%E6%9B%B4%E6%96%B0-CodePush%E6%9C%80%E6%96%B0%E9%9B%86%E6%88%90%E6%80%BB%E7%BB%93#%E9%9B%86%E6%88%90codepush-sdk)
- [CodePush热更新详细接入教程](https://www.jianshu.com/p/6a5e00d22723)
- [react-native-code-push进阶篇](https://www.jianshu.com/p/6e96c6038d80)
- [App Center](https://appcenter.ms)
- [Microsoft/react-native-code-push](https://github.com/Microsoft/react-native-code-push)