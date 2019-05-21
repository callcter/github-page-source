---
title: Node版本升级
---

### 更新步骤
1. 使用 NVM 安装新版本 NodeJS
2. NPM 查看之前安装的全局包，在新版本中安装

### 指令指南

##### NPM

```
npm list -g --depth 0 // 查看 NPM 全局安装的组件
npm i -g npm // 更新 NPM
```

##### NVM

```
nvm list // 查看已安装版本
nvm ls-remote // 查看可安装版本
nvm install v10.15.3 // 安装版本
nvm use v10.15.3 // 切换版本
nvm alias default v10.15.3 // 设置默认版本
```