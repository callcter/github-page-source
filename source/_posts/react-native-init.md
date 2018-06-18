---
title: React Native 入门，一路踩过的坑
tags:
  - react-native
url: 513.html
id: 513
categories:
  - 移动开发
date: 2017-10-17 21:30:40
---

有时候，按照入门文档一步一步地走，也会遇到各种各样的坑，React Native 有很长一段时间没有碰了，最近沉迷吃鸡，打算做一个吃鸡数据的 APP，所以又拾起来。现在的前端，真的，需要学的东西太多，并且基本上用完就放下，好累。之前没有好好记录，既然重新开始，就不要错过这么好的机会，好好记录吧

#### 环境配置

没遇到坑

#### cli 工具创建项目

*   Print: Entry, ":CFBundleIdentifier", Does Not Exist

原因：创建时有些插件没有安装上 解决办法：清空缓存，已安装文件，重装，但不需要重建项目

    rm -rf node_modules && rm -rf ~/.rncache && yarn
    

#### Hello World

*   Unhandled JS Exception: Invariant Violation: Element type is invalid: expected a string (for built-in components) or a class/function (for composite components) but got: object. **_You Likely Forgot To Export Your Component From The File It'S Defined In_**.

原因：创建的组件没有输出

    //示例代码
    ...
    class HelloWorldApp extends Component {
      ...
    }
    //修改为
    ...
    export default class HelloWorldApp extends Component {
      ...
    }
    

> 即使是官方的代码，也别信