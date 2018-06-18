---
title: node 配置 oauth2 (一) —— 密码模式
tags:
  - node
  - express
  - oauth2
url: 533.html
id: 533
categories:
  - 后台开发
date: 2018-01-07 10:45:06
---

#### 基本知识

oauth2 介绍，推荐阮老师的 [理解 oauth2](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)，深入浅出

#### 基本环境

*   使用的框架是 express
*   基本 oauth2 组件使用的是 [node-oauth2-server](https://www.npmjs.com/package/node-oauth2-server)，使用前推荐详细阅读文档，因为在配置过程中的多数错误都是因为文档阅读不仔细造成的

    npm install oauth-server
    

*   ORM 组件 sequelize

#### 过程

1.  使用 Sequelize 创建模型

    OAuthAccessToken
    OAuthClient
    OAuthRefreshToken
    

1.  oauth2 model 配置

    getClient(clientId, clientSecret, callback)
    // grantTypeAllowed (clientId, grantType, callback) 可以不添加，在oauth2server 初始化配置中使用 grants 配置
    getUser(username, password, callback)
    // validateScope(token, client, user, callback) 验证 scope，可以不添加
    saveToken(token, client, user, callback)
    

1.  express 配置

    const express = require('express')
    const oauthServer = require('oauth2-server')
    const Request = oauthServer.Request
    const Response = oauthServer.Response
    const bodyParser = require('body-parser')
    const app = express()
    const oauth = new oauthServer({
      model: require('./model'),
      grants: ['password', 'client_credentials'],
      debug: true
    })
    app.use(bodyParser.urlencoded({ extended: true }))
    app.all('/oauth/token', function(req, res, next){
      console.log(req.body)
      var request = new Request(req);
      var response = new Response(res);
      oauth.token(request, response)
        .then(function(token) {
          // Todo: remove unnecessary values in response
          return res.json(token)
        }).catch(function(err){
          return res.status(500).json(err)
        })
    })
    

1.  验证

#### 踩过的坑

*   请求 header和参数 配置 oauth2 必须使用 application/x-www-form-urlencoded，也就几组基本的 form 类型 post 请求 另外需要携带 Authorization

    headers: {
      'Authorization': 'Basic '+Base64.encode(client_id+':'+client_secret),
      'Content-Type': 'application/x-www-form-urlencoded'
    }
    

解析 x-www-form-urlencoded 格式需要在后台使用 body-parser 组件 在使用 axios 时，传递到后台的数据会被解析成字符串，所以需要使用 qs.stringify() 处理

*   error Missing paramater: grant_type 使用 qs.stringify() 处理参数

#### 源码

自己写的源码是在一个项目下的，不方便贴出来，所以推荐一个 github 上的 [node-oauth2-server-implementation](https://github.com/manjeshpv/node-oauth2-server-implementation) ，其中有个坑是 validateScope 方法落下了 user 参数，可以直接注释掉那个方法，在 model 配置部分已经说明