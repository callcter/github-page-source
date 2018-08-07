---
title: hexo 报错总结
tags:
  -hexo
---

#### 报错一：Template render error: (unknown path)
##### 原因
```
The {{}} is nunjucks syntax, therefore please avoid using them in posts
```
使用时请用代码包裹符号包裹起来
##### 依据
[github issue](https://github.com/hexojs/hexo/issues/2384)