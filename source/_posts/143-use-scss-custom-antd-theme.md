---
title: 使用 SCSS 定制 Ant Design 主题
tags:
  - React
---

### 安装插件
```
npm i -D antd-scss-theme-plugin less-loader sass-loader babel-plugin-import
```

### 创建 theme.scss 文件

文件定义自己的主题变量，例如
```
$primary-color: #1890ff;
@link-color: #1890ff;
// ...
```

### 修改 webpack.config

```
const AntdScssThemePlugin = require('antd-scss-theme-plugin')
// 添加 less 文件处理，并且使用 AntdScssThemePlugin.themify() 处理 less-loader 和 sass-loader
module: {
  rules: [
    // ...
    {
      test: /\.(less)$/,
      loaders: ['style-loader', 'css-loader', AntdScssThemePlugin.themify('less-loader')]
    },
    {
      test: /\.(scss)$/,
      loaders: ['style-loader', 'css-loader', AntdScssThemePlugin.themify('sass-loader')]
    }
    // ...
  ]
}
```

添加 plugin
```
plugins: [
  new AntdScssThemePlugin('./theme.scss')  // theme.scss 文件的路径根据个人的目录结构配置
]
```

### 修改 .babelrc

添加插件
```
"plugins": [
  ["import", {
    "libraryName": "antd",
    "style": true // 可以使用 Less 组件
  }]
]
```

### 定制主题

```scss
// theme.scss
$primary-color: #fe8019;
```
默认主题色是 #1890ff，修改为 #fe8019 后显示
![](http://cdn.dreamser.com/blue-orange-comparison.png)


### 参考文章

- [Ant Design 定制主题](https://ant.design/docs/react/customize-theme-cn)
- [antd-scss-theme-plugin README](https://www.npmjs.com/package/antd-scss-theme-plugin)
- [USING ANT DESIGN IN SASS-STYLED PROJECTS](https://intoli.com/blog/antd-scss-theme-plugin/)