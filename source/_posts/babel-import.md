---
title: Babel 配置按需加载
date: 2020/2/10 12:03:22
updated: 2020-02-10 17:29:50
tags: [HTML, CSS, JS, Babel]
categories: [Babel]
---

## Babel 配置按需加载

在使用第三方库（特别是 UI 组件库）时，我们一般采用按需加载的方式引入，即只需引入业务中需要的组件即可，未 `import` 进来的组件不会打包进来。而如果我们完整引入，这会会使 bundle 增大，进而影响应用的网络性能。

按需加载支持库：

- https://github.com/ant-design/babel-plugin-import
- https://github.com/ElementUI/babel-plugin-component

## 如何使用

下面以 `antd-mobile` 为例，演示如何通过 `babel-plugin-import` 插件进行按需引入：

```
$ npm install antd-mobile --save
```

### 完整引入

```
import { Button } from 'antd-mobile';
import 'antd-mobile/dist/antd-mobile.css';

ReactDOM.render(<Button>Start</Button>, mountNode);
```

> 你在控制台还会看到如下警告：You are using a whole package of antd-mobile, please use https://www.npmjs.com/package/babel-plugin-import to reduce app bundle size.

![image](zos.alipayobjects.com/rmsportal/GHIRszVcmjccgZRakJDQ.png)

![](user-gold-cdn.xitu.io/2020/4/24/171a8426b23c3233?w=742&h=733&f=png&s=105358)

### 按需引入

```
npm install babel-plugin-import --save-dev
```

.babelrc

```
{
  "presets": [
    ["@babel/preset-env", { "useBuiltIns": "usage", "corejs": "3" }],
    "@babel/preset-react"
  ],
  "plugins": [
    "@babel/plugin-transform-runtime",
    ["import", { "libraryName": "antd-mobile", "style": "css" }]
  ]
}
```

```
import React from 'react'

// 按需加载
// babel-plugin-import 会帮助你加载 JS 和 CSS
// import 'antd-mobile/dist/antd-mobile.css'
import { Button, Toast } from 'antd-mobile'

function showToast() {
  Toast.info('This is a toast tips !!!', 1)
}

function Mine() {
  return (
    <div className="mine-tab">
      <Button type="primary" inline>
        primary
      </Button>

      <Button onClick={showToast}>text only</Button>

      <Button icon="check-circle-o">with icon</Button>
    </div>
  )
}

export default Mine
```

你还需要在你的 webpack loader 中，把 `antd` 提供的样式文件 `include` 进来

```
include: [
  path.resolve(__dirname, '../src'),
  path.resolve(__dirname, '../node_modules/antd-mobile'),
  path.resolve(__dirname, '../node_modules/normalize.css/normalize.css'),
],
```

![](user-gold-cdn.xitu.io/2020/4/24/171a8546ccd80eb8?w=1113&h=670&f=png&s=60677)
