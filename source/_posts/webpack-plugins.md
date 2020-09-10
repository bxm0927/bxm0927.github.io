---
title: webpack 常用 plugin 收集整理
date: 2020/3/2 12:03:22
updated: 2020-03-04 17:29:50
tags: [HTML, CSS, JS, webpack]
categories: [webpack]
---

## 前言

webpack 附带了各种内置插件，可以通过 `webpack.[plugin-name]` 访问这些插件。但请注意这只是其中一部分，社区中还有许多插件。

- https://webpack.docschina.org/plugins
- 更多第三方 plugin：https://github.com/webpack-contrib/awesome-webpack#webpack-plugins

## html-webpack-plugin

- https://github.com/jantimon/html-webpack-plugin
- https://webpack.docschina.org/plugins/html-webpack-plugin

产出 HTML，处理 HTML 模版，自动更新 HTML 文件中的引入文件的 hash 值等

```
const HtmlWebpackPlugin = require('html-webpack-plugin')

plugins: [
  new HtmlWebpackPlugin({
    template: path.resolve(__dirname, 'index.html'),
  }),
],
```

## optimize-css-assets-webpack-plugin

CSS 代码压缩

https://webpack.docschina.org/plugins/mini-css-extract-plugin/#minimizing-for-production

## extract-text-webpack-plugin、mini-css-extract-plugin

- https://github.com/webpack-contrib/extract-text-webpack-plugin
- https://github.com/webpack-contrib/mini-css-extract-plugin
- https://vue-loader.vuejs.org/zh/guide/extract-css.html#webpack-4

将 JS 文件中的 CSS 提取到单独的文件中，它为每个包含 CSS 的 JS 文件创建一个 CSS 文件。建议：

- 请只在生产环境下使用 CSS 提取，这将便于你在开发环境下进行热重载；
- 而在线上环境，使样式依赖 JS 执行环境并不是一个好的实践。渲染会被推迟，甚至会出现 FOUC，因此在最终线上环境构建时，最好还是能够将 CSS 放在单独的文件中。

webpack v4 推荐使用 `mini-css-extract-plugin` 代替之。如果你一定要使用 `extract-text-webpack-plugin`，请安装`@next`版本

> npm WARN deprecated extract-text-webpack-plugin@3.0.2: Deprecated. Please use https://github.com/webpack-contrib/mini-css-extract-plugin

> npm WARN extract-text-webpack-plugin@3.0.2 requires a peer of webpack@^3.1.0 but none is installed. You must install peer dependencies yourself.

```
# webpack 3
npm install --D extract-text-webpack-plugin

# webpack 4（不推荐）
npm install --D extract-text-webpack-plugin@next

# webpack 4（推荐）
npm install --D mini-css-extract-plugin
```

## webpack.optimize.UglifyJsPlugin、UglifyJsPlugin

JS 文件合并压缩等处理

在 webpack4.x 下，只需要配置 `mode` 为 `production` 即可。

![image](https://note.youdao.com/yws/res/127326/679DFFE53F144CE3AFD68761F0B28049)

## webpack.optimize.CommonsChunkPlugin、SplitChunksPlugin

https://webpack.docschina.org/plugins/split-chunks-plugin/

> CommonsChunkPlugin 已经从 webpack v4 中移除。想要了解最新版本是如何处理 chunk，请查看 SplitChunksPlugin。

SplitChunksPlugin 插件可以将公共的依赖模块提取到已有的 entry chunk 中，或者提取到一个新生成的 chunk。

## clean-webpack-plugin

https://github.com/johnagan/clean-webpack-plugin

用于在构建之前删除构建文件夹(`/dist`)

```
npm install --save-dev clean-webpack-plugin
```

```
const { CleanWebpackPlugin } = require('clean-webpack-plugin')

plugins: [
  new CleanWebpackPlugin(),
],
```

## copy-webpack-plugin

https://github.com/webpack-contrib/copy-webpack-plugin

将单个文件或整个目录复制到构建目录

```
// Copy custom static assets
new CopyWebpackPlugin([
  {
    from: path.resolve(__dirname, '../static'),
    to: 'static',
  },
]),
```

## webpack.DefinePlugin

允许在编译时(compile time)配置的全局常量，通常用来设置环境变量

```js
const webpack = require('webpack')

plugins: [
  new webpack.DefinePlugin({
    'process.env': {
      NODE_ENV: JSON.stringify(process.env.NODE_ENV),
    },
  }),
],
```

```js
"scripts": {
  "build": "cross-env NODE_ENV=production webpack",
  "dev": "cross-env NODE_ENV=development webpack-dev-server"
},
```

## webpack.ProvidePlugin

处理引用的第三方库，暴露全局变量

```
new webpack.ProvidePlugin({
  _: 'lodash',
  $: 'jquery'
})
```

## workbox-webpack-plugin

PWA 支持

简言之：在你第一次访问一个网站的时候，如果成功，做一个缓存，当服务器挂了之后，你依然能够访问这个网页 ，这就是 PWA。那相信你也已经知道了，这个只需要在生产环境，才需要做 PWA 的处理，以防不测。

```
cnpm i workbox-webpack-plugin -D
```

```
const WorkboxPlugin = require('workbox-webpack-plugin')

const prodConfig = {
  plugins: [
    new WorkboxPlugin.GenerateSW({
      clientsClaim: true,
      skipWaiting: true
    })
  ]
}
```

```
// 在入口文件加上
// 判断该浏览器支不支持 serviceWorker
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker
      .register('/service-worker.js')
      .then(registration => {
        console.log('service-worker registed')
      })
      .catch(error => {
        console.log('service-worker registed error')
      })
  })
}
```
