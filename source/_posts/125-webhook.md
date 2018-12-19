---
title: 使用 webhook 配置自动部署指南（coding.net）
tags:
  - linux
categories:
  - 服务器
---

#### webhook自动部署指南


##### 部署服务器

```javascript
const http = require('http')
const exec = require('child_process').exec
const createHandler = require('coding-webhook-handler')

const handler = createHandler({
  path: '/'
})

handler.on('error', function(err){
  console.error('Error:' + err)
})

handler.on('push', function(event){
  console.log(
    'Received a push event for %s to %s',
    event.payload.repository.name,
    event.payload.ref
  )
  runCommand('sh ./deploy.sh')
})

http.createServer(function(req, res){
  handler(req, res, function(err){
    res.statusCode = 404
    res.end('no such location')
  })
}).listen(4000)

function runCommand(content){
  exec(content, function(error, stdout, stderr){
    if(error){
      console.error('exec error:' + error)
      return
    }
    console.log('stdout:' + stdout)
    console.log('stderr:' + stderr)
  })
}
```

##### 部署脚本

```shell
#! /bin/bash
cd /home/www/station

git reset --hard origin/master && git clean -f
git pull origin master

npm install
npm run start
```

##### 项目 package.json scripts

```
forever restart --minUptime 1000ms --spinSleepTime 1000ms app.js
```