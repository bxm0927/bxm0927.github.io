---
title: webpack 多页应用动态入口配置
date: 2020/3/7 18:00:10
updated: 2020-03-12 17:10:50
tags: [HTML, CSS, JS, webpack]
categories: [webpack]
---

## 原理

使用 fs、glob 或 shelljs 等可以读取文件系统的库，解析出项目中所有的入口文件，然后动态生成 `entry` 和 `HtmlWebpackPlugin`，这样就不要每次新增页面就手动修改 webpack entry。

- http://nodejs.cn/api/fs.html
- https://github.com/isaacs/node-glob
- https://github.com/shelljs/shelljs

## 使用 glob 实现

![](user-gold-cdn.xitu.io/2020/5/7/171eb27781a195fe?w=1402&h=985&f=png&s=183986)

```js
// Multi-entry configuration
const getEntries = () => {
  let entry = {}
  let htmlWebpackPlugin = []
  const entries = glob.sync('./src/pages/**/index.js') // Pay attention to rules
  console.log('entries: ', entries)

  entries.forEach((path) => {
    const pageName = path.slice('./src/pages/'.length, -'/index.js'.length)
    entry[pageName] = path

    // Something like this:
    // entry:  {
    //   index: './src/pages/index/index.js',
    //   page1: './src/pages/page1/index.js',
    //   page2: './src/pages/page2/index.js',
    //   page3: './src/pages/page3/index.js',
    //   'page3/page3-child': './src/pages/page3/page3-child/index.js'
    // }

    const html = new HtmlWebpackPlugin({
      template: 'index.html', // Use the same template
      filename: `${pageName}.html`,
      title: `${pageName} Page`,
      chunks: ['common', pageName],
    })
    htmlWebpackPlugin.push(html)
  })

  return { entry, htmlWebpackPlugin }
}

const { entry, htmlWebpackPlugin } = getEntries()

module.exports = {
  entry,
  // ...
  plugins: [...htmlWebpackPlugin],
}
```

## 使用 shelljs 实现

```
shelljs.ls(path.join(__dirname, '/src/**/*.page.js')).forEach((f) => {
    // ...
})
```

示例 1

```
const shelljs = require('shelljs')

/**
 * 通过约定的入口文件命名构建 webpack entry：`模块名.页面名`
 * 过程解析：
 * - filePath: `path/to/src/pages/animal/animal.page.js`
 * - filePathArr: `['path', 'to', 'src', 'pages', 'animal', 'animal.page.js']`
 * - fileName: `animal.page.js`
 * - pageName: `animal`
 * - entryFile: `./src/pages/animal/animal.page.js`
 * - template: `./src/pages/animal/index.html`
 * - entry: `{ animal: './src/pages/animal/animal.page.js' }`
 */
let entry = {}
let htmlWebpackPlugin = []
const entries = shelljs.ls(path.join(__dirname, './src/pages/**/*.page.js'))

entries.forEach(filePath => {
  const filePathArr = filePath.split('/')
  const fileName = _.last(filePathArr)
  const pageName = fileName.replace(/\.page.js$/, '')

  const srcPos = _.indexOf(filePathArr, 'src')
  const entryFile = `./${filePathArr.slice(srcPos).join('/')}`
  const template = entryFile.replace(fileName, `${pageName}.html`)

  entry[pageName] = entryFile

  const html = new HtmlWebpackPlugin({
    template,
    // template: path.resolve(__dirname, `./src/pages/${pageName}/${pageName}.html`),
    filename: `${pageName}.html`,
  })
  htmlWebpackPlugin.push(html)
})
```

示例 2

```
const shelljs = require('shelljs')

let entry = {}

// 通过约定的入口文件命名构建webpack打包入口
// entry的name规则为：模块名.页面名
shelljs.ls(path.join(__dirname, '/source/**/*.page.js')).forEach((filePath) => {
  let filePathArr = filePath.split('/')

  let file = _.last(filePathArr)
  let page = file.replace(/\.page.js$/, '') // 从文件名中取出需要打包的文件的「页面名」

  // 「模块名」取/source/module/，即source目录下一级目录名
  let pos = _.indexOf(filePathArr, 'source')
  let mod = filePathArr.slice(pos + 1, pos + 2)

  // 防止直接将.page.js放到source目录中
  if (/\.page.js$/.test(mod)) {
    console.log(err(`Please don't settle ${chalk.bold(file)} in folder source directly`))
    return
  }

  entry[`${mod}.${page}`] = `./${filePathArr.slice(pos).join('/')}`
})
```

## 使用 fs 实现

```js
const fs = require('fs')
const path = require('path')

const MAIN_FILE = 'index.js'
const TEMPLATE_FILE = 'index.html'
const DIST_DIR = path.resolve(__dirname, '../dist')
const SRC_DIR = path.resolve(__dirname, '../src')
const PAGES_DIR = path.resolve(SRC_DIR, 'pages')
const DEV_MODE = process.env.NODE_ENV !== 'production'

// Multi-entry configuration
const getEntries = () => {
  let entrys = {}

  fs.readdirSync(PAGES_DIR).forEach((pageName) => {
    const fullPagePath = path.resolve(PAGES_DIR, pageName)
    const fullFilePath = path.resolve(fullPagePath, MAIN_FILE)
    const status = fs.statSync(fullPagePath)

    if (status.isDirectory() && fs.existsSync(fullFilePath)) {
      entrys[pageName] = fullFilePath
    }
  })

  return entrys
}

// Multi-template configuration
const getTemplates = (entrys) => {
  let htmlWebpackPlugin = []

  Object.keys(entrys).forEach((pageName) => {
    const fullPagePath = path.resolve(PAGES_DIR, pageName)
    const fullFilePath = path.resolve(fullPagePath, TEMPLATE_FILE)
    const status = fs.statSync(fullPagePath)

    if (status.isDirectory() && fs.existsSync(fullFilePath)) {
      const html = new HtmlWebpackPlugin({
        filename: `${pageName}.html`,
        template: fullFilePath,
        chunks: [pageName],
        title: `Page ${pageName}`,
      })
      htmlWebpackPlugin.push(html)
    }
  })

  return htmlWebpackPlugin
}

const entrys = getEntries()
const htmlWebpackPlugin = getTemplates(entrys)

module.exports = {
  entry: entrys,
  // ...
  plugins: [
    // creation of HTML files
    ...htmlWebpackPlugin,
  ],
}
```
