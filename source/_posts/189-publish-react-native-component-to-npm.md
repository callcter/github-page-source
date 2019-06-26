---
title: '发布 ReactNative 组件到 NPM'
---

## 截屏捕捉组件
```
npm i react-native-screenshotcatch
```

## 背景

前一段时间写的 react-native 截屏监听功能是直接写进项目中的，现在打算将其单独封装发布到 NPM 上去，之前也发过一个命令行工具但是没有写文记录一下过程，这次补上

<!-- more -->

## 实现

#### 使用 react-native-create-library

`react-native-create-library` 是一个自定义组件模板工具，比自己去创建省很多麻烦

```shell
npm i -g react-native-create-library # 安装
react-native-create-library --package-identifier com.dreamser.screenshotcatch --platforms android,ios screenshotcatch
mv screenshotcatch react-native-screenshotcatch
```

#### 更新 build.gradle

`react-native-create-library` 中的依赖版本太低，需要更新一下

#### 编写代码

请见 [React Native 实现截图添加二维码分享功能](http://liu-hang.cn/2019/06/11/185-react-native-screen-shot-share/)

#### 完善 readme

`react-native-create-library` 已经为你准备了基础的模板，具体内容需要自己来添加

#### 上传 github

初始化本地repo，在github创建远程repo，连接，上传代码...在这里不做赘述

#### 完善 package.json

package.json文件定义了发布的所有信息，包括：组件名、版本、作者、描述、依赖等等关键信息。具体可以参照 [Creating a package.json file](https://docs.npmjs.com/creating-a-package-json-file)

```json

{
  "name": "react-native-screenshotcatch",
  "version": "0.0.1",
  "description": "A ReactNative Tool for catch Screen Shot Event",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "react-native",
    "screen-shot",
    "screen-capture",
    "ios",
    "android"
  ],
  "author": {
    "name": "callcter",
    "email": "sharpliuzigu@gmail.com"
  },
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "git@github.com:callcter/react-native-screenshotcatch.git"
  },
  "peerDependencies": {
    "react-native": "^0.41.2"
  }
}
```

#### 发布到 NPM

```shell
npm adduser # 创建 NPM 账号
npm login # 如果已经在官网有账号，可以直接登录
npm whoami # 查看登录状态
npm publish # 发布
```

发布时可能遇到 `publish Failed PUT 403` 错误，原因是更改了镜像源

```shell
npm config get registry # 查看当前镜像源
npm config set registry=http://registry.npmjs.org # 更改回官方镜像源
npm publish # 重新发布
```

#### NPM 更新

npm 提供官方提供了`npm version`来进行版本控制，其效果跟手动修改package.json里面的version字段是一样的，好处在于，可以在构建过程中用`npm version`命令自动修改，而且具有语义化即`Semantic versioning`

```
npm version [<newversion> | major | minor | patch | premajor | preminor | 
prepatch | prerelease | from-git]
```

语义为：

```
major：主版本号（大版本）
minor：次版本号（小更新）
patch：补丁号（补丁）
premajor：预备主版本
preminor: 预备次版本
prepatch：预备补丁版本
prerelease：预发布版本
```

## 参考文章

- [开发自己的react-native组件并发布到npm](https://blog.csdn.net/sinat_17775997/article/details/82491712)
- [frostney/react-native-create-library](https://github.com/frostney/react-native-create-library)
- [NPM包（模块）发布、更新、撤销发布](https://juejin.im/post/5c5012926fb9a049d37f81e1)