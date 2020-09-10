---
title: webpack 常用 loader 收集整理
date: 2020/3/1 13:02:20
updated: 2020-03-03 17:20:52
tags: [HTML, CSS, JS, webpack]
categories: [webpack]
---

## webpack loader 简介

webpack 可以使用 loader 来预处理文件。这允许你打包除 JavaScript 之外的任何静态资源。你可以使用 Node.js 来很简单地编写自己的 loader。

webpack 提供许多开箱可用的 loader：

- https://webpack.docschina.org/loaders/
- 更多第三方 loader：https://github.com/webpack-contrib/awesome-webpack#loaders

## 样式

### style-loader、css-loader、postcss-loader

- https://github.com/webpack-contrib/css-loader

处理 CSS 文件

- style-loader 把 CSS 文件转换成 `<style>` 标签，插入到 `<head>` 中
- css-loader 用来处理 CSS 中的 URL 路径
- postcss-loader 用于 CSS 后处理，添加浏览器前缀等

> 注意：打包顺序是从后往前依次执行的

```
npm install -D style-loader css-loader postcss-loader autoprefixer
```

```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const devMode = process.env.NODE_ENV !== 'production'

// webpack.config.js -> module.rules
{
  // test: /\.css$/,
  // use: ['style-loader', 'css-loader', 'postcss-loader'], // 不再需要 style-loader
  test: /\.(sa|sc|c)ss$/,
  use: [
    {
      loader: MiniCssExtractPlugin.loader,
      options: {
        // you can specify a publicPath here
        // by default it uses publicPath in webpackOptions.output
        hmr: process.env.NODE_ENV === 'development',
      },
    },
    'css-loader',
    'postcss-loader',
    'sass-loader',
  ],
  include: path.resolve(__dirname, 'src'),
},

// webpack.config.js -> module.plugins
new MiniCssExtractPlugin({
  // Options similar to the same options in webpackOptions.output
  // both options are optional
  filename: devMode ? '[name].css' : '[name].[hash].css',
  chunkFilename: devMode ? '[id].css' : '[id].[hash].css',
}),
```

postcss.config.js

```js
module.exports = {
  plugins: [require('autoprefixer')], // 添加浏览器前缀
}
```

### sass-loader

转换 sass/scss 文件为 css 文件

```
npm install --D sass-loader node-sass
```

注意 sass-loader 会默认处理不基于缩进的 scss 语法。为了使用基于缩进的 sass 语法，你需要向这个 loader 传递选项：

```js
// webpack.config.js -> module.rules
{
  test: /\.sass$/,
  use: [
    'vue-style-loader',
    'css-loader',
    {
      loader: 'sass-loader',
      options: {
        indentedSyntax: true
      }
    }
  ]
}
```

### less-loader

转换 less 文件为 css 文件

```
npm install -D less less-loader
```

### stylus-loader

转换 stylus 文件为 css 文件

```
npm install -D stylus stylus-loader
```

```
// webpack.config.js -> module.rules
{
  test: /\.styl(us)?$/,
  use: [
    'vue-style-loader',
    'css-loader',
    'stylus-loader'
  ]
}
```

### PostCSS

- https://github.com/postcss/postcss-loader
- https://github.com/postcss/autoprefixer

PostCSS 的配置可以通过 postcss.config.js 或 postcss-loader 选项来完成。

```
npm install -D postcss-loader
```

### sass-resources-loader / style-resources-loader

- https://github.com/shakacode/sass-resources-loader
- https://github.com/yenshih/style-resources-loader
- Nuxt 配置：https://nuxtjs.org/api/configuration-build#styleresources

在所有样式中共享变量（variables）和混合（mixins），而无需在每个文件中手动导入它们。

## 转译

### babel-loader

- https://babel.docschina.org/
- https://github.com/babel/babel-loader
- https://webpack.docschina.org/loaders/babel-loader

ES6 转 ES5

> babel-loader@8 requires Babel 7.x (the package '@babel/core'). If you'd like to use Babel 6.x ('babel-core'), you should install 'babel-loader@7'.

```bash
# webpack 4.x | babel-loader 8.x | babel 7.x
npm install -D babel-loader @babel/core @babel/preset-env webpack
```

```
{
  test: /\.js$/,
  loader: 'babel-loader',
  exclude: /node_modules/,
},
```

Babel 的配置可以通过 `.babelrc` 文件或 babel-loader 的 `options` 选项来完成，如：

```
{
  "presets": ["@babel/preset-env"],
  "plugins": ["@babel/plugin-transform-runtime"]
}
```

### ts-loader

https://github.com/TypeStrong/ts-loader

解析 TypeScript 文件（`.ts`）

TypeScript 的配置可以通过 `tsconfig.json` 来完成。

```
npm install -D typescript ts-loader
```

```
// webpack.config.js
module.exports = {
  resolve: {
    extensions: [ '.tsx', '.ts', '.js' ]
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        loader: 'ts-loader',
        exclude: /node_modules/
        options: {
          appendTsSuffixTo: [/\.vue$/]
        }
      }
    ]
  },
}
```

tsconfig.json：

```
{
  "compilerOptions": {
    "outDir": "./dist/",
    "noImplicitAny": true,
    "module": "es6",
    "target": "es5",
    "jsx": "react",
    "allowJs": true
  }
}
```

## 框架

### vue-loader

- https://github.com/vuejs/vue-loader
- https://vue-loader.vuejs.org/zh/

vue-loader 用于加载和转译 Vue 单文件组件（SFC，Single-File Components）

```
npm install --D vue-loader vue-template-compiler
```

```
const { VueLoaderPlugin } = require('vue-loader')

module.exports = {
  module: {
    rules: [
      // ... 其它规则
      {
        test: /\.vue$/,
        loader: 'vue-loader'
      }
    ]
  },
  plugins: [
    // 这个插件是必须的！
    // 它的职责是将你定义过的其它规则复制并应用到 .vue 文件里相应语言的块。
    // 例如，如果你有一条匹配 /\.js$/ 的规则，那么它会应用到 .vue 文件里的 <script> 块。
    new VueLoaderPlugin()
  ]
}
```

### 支持 react

```
npm install --D @babel/preset-react
```

```
// module.rules
{
  test: /\.jsx?$/,
  exclude: /node_modules/,
  use: ["babel-loader"]
},

```

.babelrc：

```
{
  "presets": [
    [
      "@babel/preset-react",
      {
        "pragma": "dom", // default pragma is React.createElement
        "pragmaFrag": "DomFrag", // default is React.Fragment
        "throwIfNamespace": false // defaults to true
      }
    ]
  ]
}
```

index.js：

```
import React, { Component } from 'react'
import ReactDOM from 'react-dom'

class App extends Component {
  render() {
    return <div>react frame content.</div>
  }
}

ReactDOM.render(<App />, document.getElementById('root'))
```

## 文件

### file-loader

==file-loader 将文件发送到输出文件夹，并返回（相对）URL==

`file-loader` 可以指定要复制和放置资源文件的位置，以及如何使用版本哈希命名以获得更好的缓存。此外，这意味着你可以就近管理图片文件，可以使用相对路径而不用担心部署时 URL 的问题。使用正确的配置，webpack 将会在打包输出中自动重写文件路径为正确的 URL。

### url-loader

==url-loader 像 file loader 一样工作，但如果文件小于限制，可以返回 data URL==

`url-loader` 允许你有条件地将文件转换为内联的 base-64 URL (当文件小于给定的阈值)，这会减少小文件的 HTTP 请求数。如果文件大于该阈值，会自动的交给 `file-loader` 处理。

```
npm install --D file-loader url-loader
```

```
// images
{
  test: /\.(png|jpe?g|gif|webp)(\?.*)?$/,
  use: [
    {
      loader: 'url-loader',
      options: {
        limit: 8 * 1024, // 8K
        outputPath: 'img/',
        name: devMode ? '[name].[ext]' : '[name].[hash:8].[ext]',
      },
    },
    'image-webpack-loader',
  ],
},

// svg
{
  test: /\.(svg)(\?.*)?$/,
  use: {
    loader: 'file-loader',
    options: {
      outputPath: 'img/',
      name: devMode ? '[name].[ext]' : '[name].[hash:8].[ext]',
    },
  },
},

// fonts
{
  test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/i,
  use: {
    loader: 'url-loader',
    options: {
      limit: 8 * 1024, // 8K
      outputPath: 'fonts/',
      name: devMode ? '[name].[ext]' : '[name].[hash:8].[ext]',
    },
  },
},

// media
{
  test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/,
  use: {
    loader: 'url-loader',
    options: {
      limit: 8 * 1024, // 8K
      outputPath: 'media/',
      name: devMode ? '[name].[ext]' : '[name].[hash:8].[ext]',
    },
  },
},
```

### image-webpack-loader

https://github.com/tcoopman/image-webpack-loader

压缩图片

```
{
  test: /\.(png|jpe?g|gif|svg)$/,
  use: [
    {
      loader: 'url-loader',
      options: {
        limit: 8 * 1024, // 8K
        outputPath: 'assets/',
        name: 'images/[name].[hash].[ext]',
      },
    },
    'image-webpack-loader',
  ]
}
```

### csv-loader

```
{
  test: /\.(csv|tsv)$/,
  use: ['csv-loader']
}
```

### xml-loader

```
{
  test: /\.xml$/,
  use: ['xml-loader']
}
```

## 模板

### ejs-loader

处理 ejs 模版引擎文件

### jade-loader

对 `.jade` 文件使用 jade-loader

### pug-plain-loader

https://vue-loader.vuejs.org/zh/guide/pre-processors.html#pug

处理 pug 模版引擎文件

```
npm install -D pug pug-plain-loader
```

```
// webpack.config.js -> module.rules
{
  test: /\.pug$/,
  loader: 'pug-plain-loader'
}
```

## 代码检查和测试

### eslint-loader

你的 `*.vue` 文件在开发过程中每次保存的时候就会自动进行代码校验

```
npm install -D eslint eslint-loader
```

请确保它是作为一个 pre-loader 运用的：

```
// webpack.config.js
module.exports = {
  // ... 其它选项
  module: {
    rules: [
      {
        enforce: 'pre',
        test: /\.(js|vue)$/,
        loader: 'eslint-loader',
        exclude: /node_modules/
      }
    ]
  }
}
```
