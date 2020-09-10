---
title: 🎉⛓️🛠️ 使用 ESLint + Stylelint + Prettier，打造舒适前端开发体验
date: 2020/09/03 23:22:00
updated: 2020-09-03 23:29:50
tags: [ESLint, Stylelint, Prettier]
categories: [ESLint]
---

## ESLint 简介

- https://github.com/eslint/eslint
- https://cn.eslint.org/
- https://github.com/dustinspecker/awesome-eslint

不管是多人协作还是个人项目，代码规范都是很重要的。这样做不仅可以很大程度地避免基本语法错误，也保证了代码的可读性。具备基本工程素养的同学都会注重编码规范，而代码风格检查（Code Linting，简称 Lint）是保障代码规范一致性的重要手段。

JavaScript 是一个动态的弱类型语言，在开发中比较容易出错。因为没有编译程序，为了寻找 JavaScript 代码错误通常需要在执行过程中不断调试。像 ESLint 这样的工具，可以让程序员在编码的过程中发现问题而不是在执行的过程中。

==ESLint 是用于查找并修复 JavaScript 代码中的问题的代码检测工具==。它是由 Nicholas C. Zakas（“红宝书”《JavaScript 高级程序设计》的作者）于 2013 年 6 月创建的开源项目，ESLint 的所有规则都被设计成可插拔的，它的目标是提供一个插件化的 JavaScript 代码检测工具，进而保证代码风格的一致性和语法正确。

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7ffaefc9829945f4b119756cea86adb9~tplv-k3u1fbpfcp-zoom-1.image)

## 安装和使用

先决条件：Node.js >= 10.12

```bash
# Local Installation
$ npm install eslint --save-dev

# set up a configuration file（.eslintrc）:
$ ./node_modules/.bin/eslint --init

# After that, you can run ESLint on any file or directory like this:
$ ./node_modules/.bin/eslint yourfile.js
```

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/83ca58069c3844c3a88b8315aa92439e~tplv-k3u1fbpfcp-zoom-1.image)

## 配置（Configuration）

### 配置文件格式（Formats）

ESLint 支持几种格式的配置文件，如果同一个目录下有多个配置文件，ESLint 只会使用一个，优先级从上到下：

- `eslintrc.js`
- `.eslintrc.yaml`
- `.eslintrc.yml`
- `.eslintrc.json`
- `.eslintrc`
- `package.json` 中的 `eslintConfig` 属性

==在官方文档中，`.eslintrc` 已经被弃用，推荐使用 `.eslintrc.js`。==

```
module.exports = {
  parser: {},  // 解析器
  extends: [], // 继承的规则 [扩展]
  plugins: [], // 插件
  rules: {}    // 规则
};
```

### 配置忽略文件（Ignoring Files）

你可以通过在项目根目录创建一个 `.eslintignore` 文件告诉 ESLint 去忽略特定的文件和目录。

ESLint 总是忽略 `/node_modules/*` 和 `/bower_components/*` 中的文件，你无需自己指定。

```
# /node_modules/* and /bower_components/* in the project root are ignored by default

# Ignore built files
build/*

# Ignore Idea files
.idea

# Ignore Nuxt built files
.nuxt

# Ignore iconfont file
assets/fonts/iconfont.js
```

### 指定解析器（Parser）

ESLint 默认使用 Espree 作为其解析器，你可以在配置文件中指定一个不同的解析器，以便让 ESLint 在处理非 ECMAScript 5 特性时正常工作。

- babel-eslint：用于 ESLint 的 Babel 解析器的包装器
  - https://github.com/babel/babel-eslint
- typescript-eslint-parser：用于 ESLint 的 TS 解析器
  - https://github.com/eslint/typescript-eslint-parser

```
"parser": "babel-eslint"
```

### 指定环境（Environments）

指定脚本的运行环境，每种环境都有一组特定的预定义全局变量。

常见环境：`browser`、`node`

```
{
    "env": {
        "browser": true,
        "node": true
    }
}
```

### 指定全局变量（Globals）

当你使用一些 ESLint 不能识别的全局变量，`no-undef` 规则将发出警告，你可以使用注释或在配置文件中定义这些全局变量。

```
{
    "globals": {
        "BMap": "readonly"
    }
}
```

### 配置注释（Comments）

你也在文件中，对指定的规则启用或禁用规则：

```
/* eslint-disable no-alert, no-console */

alert('foo');
console.log('bar');

/* eslint-enable no-alert, no-console */
```

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dc7384b183f748218a79417e78764283~tplv-k3u1fbpfcp-zoom-1.image)

## 规则（rules）

ESLint 的核心就是其中包含的各种规则（rules），这些规则大多为众多开发者经验的结晶：

- 有的可以帮我们避免错误；
- 有的可以帮我们规范代码格式；
- 有的可以帮我们规范变量的使用方式；
- 有的可以帮我们写出最佳实践的代码；
- 用的可以帮我们更合适的使用新的语法；
- …

你可以在配置文件 `.eslintrc.{js,yml,json}` 中看到许多像这样的规则：

```
{
    "rules": {
        "semi": ["error", "always"],
        "quotes": ["error", "double"]
    }
}
```

`"semi"` 和 `"quotes"` 是 ESLint 中规则的名称。第一个值是规则的错误级别，可以是以下值之一，这三个错误级别使您可以细粒度控制 ESLint 如何应用规则。

- `"off"` 或 `0` - 关闭规则
- `"warn"` 或 `1` - 将规则作为警告打开（不影响退出代码）
- `"error"` 或 `2` - 将规则作为错误打开（退出代码将为 1）

ESLint 附带有大量的规则，你可以根据需要启用额外的规则。常见规则：

- semi：末尾分号
- quotes：双引号或单引号
- indent：缩进
- no-console：禁用 `console`
- no-alert：禁用 `alert`
- no-empty：禁止空语句块
- no-var：要求使用 `let` 或 `const`，而不是 `var`
- no-undef：禁止使用未声明的变量，除非它们在 global 中被提到
- no-unused-vars：定义了变量，却没有使用
- no-magic-numbers：禁用魔术数字
- eqeqeq：要求使用 `===` 和 `!==`
- curly：当代码块只有一条语句时，是否省略大括号
- arrow-parens：要求箭头函数的参数使用圆括号
- linebreak-style：预期的换行符为 `LF`，但发现 `CRLF` 换行式

## 继承（extends）

ESLint 默认不会去校验你的代码，所有的规则默认都是禁用的，只有在你的配置文件中继承了一个配置，或者明确开启一个规则，ESLint 才会去校验你的代码。

==extends 提供的是 ESLint 现有规则的一系列预设==。

extends 可以是一个字符串，也可以是一个数组。其中可以包含以下内容：

- 以 `eslint:` 开头的字符串，如 `eslint:recommended`，这样写意味着使用 ESLint 的推荐配置；
- 以 `plugin:` 开头的字符串，如 `plugin:react/recommended`，这些写意味着使用第三方插件；
- 以 `eslint-config-` 开头的包，其实是第三方规则的集合，
- 由于 ESLint 中添加了额外的处理，我们可以省略包名的前缀 `eslint-config-` 和 `eslint-plugin-`。

您还可以在 npmjs.com 上搜索 **`eslint-config-`**，可以发现大量的 ESLint 拓展配置模块，我们可以直接通过这些模块在 ESLint 中使用上流行的风格，也可以把自己的配置结果封装为一个模块，供之后复用。

示例：

```
extends: [
  // add more generic rulesets here
  'eslint:recommended',
  'plugin:vue/recommended',
  'plugin:prettier/recommended',
],
```

## 插件（plugins）

ESLint 支持使用第三方插件来扩展规则集。在使用插件之前，你必须使用 npm 安装它。

==plugin 可以看作是 **第三方规则** 的集合==，ESLint 本身规则只会去支持 **标准的 ECMAScript** 语法，但是比如我们想在 React 中也使用 ESLint 则需要自己去定义一些规则，这就有了 eslint-plugin-react 。

您还可以通过在 npmjs.com 上搜索 **`eslint-plugin-`** 来使用其他人创建的插件。

示例：

```
module.exports = {
  parser: '@typescript-eslint/parser',
  extends: [
    'plugin:@typescript-eslint/recommended',
    'react-app',
    'plugin:prettier/recommended',
  ],
  plugins: [
    '@typescript-eslint',
    'react',
  ],
}
```

## 集成（Integrations）

### 在 VS Code 中使用 ESLint

> VSCode 中使用 ESlint 和 prettier 的正确姿势：https://zhuanlan.zhihu.com/p/159426292

https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint

在 VS Code 插件市场中安装 ESLint 插件，安装即可。

```
// 保存代码时，根据 ESLint 规则自动修复代码
"editor.codeActionsOnSave": {
  "source.fixAll.eslint": true,
},
```

### 与 Webpack 集成

eslint-loader：https://github.com/webpack-contrib/eslint-loader

```
npm install --save-dev eslint
npm install --save-dev eslint-loader
```

```js
module.exports = {
  module: {
    rules: [
      {
        // Run ESLint on save
        enforce: 'pre',
        test: /\.(js|vue)$/,
        loader: 'eslint-loader',
        exclude: /node_modules/,
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
      },
    ],
  },
}
```

### 与 Gulp 集成

gulp-eslint：https://github.com/adametry/gulp-eslint

### 与 Prettier 集成

eslint-plugin-prettier：https://github.com/prettier/eslint-plugin-prettier

- `eslint-config-prettier` 关闭所有不必要的或可能与 Prettier 冲突的 ESLint 规则。
- `eslint-plugin-prettier` 是一个 ESLint 插件，它添加了使用 Prettier 格式化内容的规则，并提供了一个推荐配置(recommend)。

```
npm install --save-dev eslint
npm install --save-dev --save-exact prettier # 安装精确的版本
npm install --save-dev eslint-config-prettier eslint-plugin-prettier
```

```
"extends": [
    'eslint:recommended',
    'plugin:prettier/recommended',
],
```

### 与 Vue 集成

eslint-plugin-vue

- version 7：https://github.com/vuejs/eslint-plugin-vue
- version 6.2.2：https://github.com/vuejs/eslint-plugin-vue/blob/v6.2.2/docs/user-guide/README.md

```sh
npm install --save-dev eslint
npm install --save-dev babel-eslint
npm install --save-dev eslint-plugin-vue # 安装 6.2.2 版本
```

```js
const isDev = process.env.NODE_ENV !== 'production'

module.exports = {
  root: true,
  env: {
    browser: true,
    node: true,
  },
  parser: 'vue-eslint-parser',
  parserOptions: {
    parser: 'babel-eslint',
  },
  plugins: ['vue'],
  extends: [
    // add more generic rulesets here
    'eslint:recommended',
    'plugin:vue/recommended',
    'plugin:prettier/recommended',
  ],
  rules: {
    // override/add rules settings here
    'no-console': isDev ? 'warn' : 'error',
    'no-debugger': isDev ? 'warn' : 'error',

    // vue rules
    'vue/no-v-html': 'off',
    'vue/attributes-order': 'off',
    'vue/max-attributes-per-line': 'off',
  },
}
```

### 与 Nuxt 集成

eslint-plugin-nuxt：https://github.com/nuxt/eslint-plugin-nuxt

在 Nuxt.js 中集成 ESLint：https://zh.nuxtjs.org/guide/development-tools/#eslint

### 与 React 集成

eslint-plugin-react：https://github.com/yannickcr/eslint-plugin-react

```
npm install --save-dev eslint
npm install --save-dev eslint-loader
npm install --save-dev babel-eslint
npm install --save-dev eslint-plugin-react
```

```js
module.exports = {
  root: true,
  env: {
    browser: true,
    node: true,
  },
  parser: 'babel-eslint',
  extends: ['eslint:recommended', 'plugin:react/recommended'],
  plugins: ['react'],
  settings: {
    react: {
      // "detect" automatically picks the version you have installed.
      version: 'detect',
    },
  },
  rules: {
    'react/prop-types': 'off',
  },
}
```

### 与 TypeScript 集成

```
npm install --save-dev eslint
npm install --save-dev typescript @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

```
"parserOptions": {
    "parser": "@typescript-eslint/parser",
},
"extends": [
    'plugin:@typescript-eslint/recommended',
    'plugin:prettier/recommended',
],
"plugins": [
    "@typescript-eslint"
],
```

## Git Hooks 强制校验

上面这些静态校验都依赖于程序员个人的自觉性，并不是强制的，有些团队成员或者说刚来的实习生没有在编辑器中配置 ESLint 或者无视命令行中提示的错误，所以还是有可能将错误的代码提交到仓库中。

如果要做到强制校验，最有效的解决方案就是搭配 Git `pre-commit` hooks 插件，来在 `git commit` 前强制校验，进而保证所有提交到远程仓库的内容都是符合团队规范的。

- husky：https://github.com/typicode/husky
- lint-staged：https://github.com/okonet/lint-staged
- pre-commit：https://github.com/observing/pre-commit

husky 可以在 `git commit` 和 `git push` 前执行静态校验，但是他会校验所有文件，当项目大了之后，检查速度会变得越来越慢。这时候就需要使用 lint-staged，它在 git 暂存文件上运行 linters，只会校验你将要提交的内容。

pre-commit 与 husky 类似，也可以指定要运行的 scripts 以及运行的顺序，这里使用 `husky + lint-staged` 即可。

```
npm install husky lint-staged --save-dev
```

然后修改 package.json，增加配置：

```json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{js,vue}": "eslint",
    "*.{css,vue}": "stylelint"
  }
}
```

如上配置，每次当你执行 `git commit` 时，都会校验你提交的内容是否符合项目配置的 ESLint 规则，即对所有的 js 和 vue 文件运行 `eslint` 指令，如果符合规则，则会提交成功；如果不符合规则，则会提交失败，并且提示你错误原因。

如果你希望在不符合规则时，让它尝试帮你自动修复，你可以将 `eslint` 指令替换成 `eslint --fix`，这样，如果修复成功则会帮你把修复好的代码提交，如果修复失败，则会提示你错误，让你修好这个错误之后才能允许你提交代码。

## 最佳实践（Best Practices）

这里推荐使用 ESLint + Prettier + Airbnb

- Linters 注重于 Code-quality rules，我们使用 ESLint 以捕获可能的错误。
- Prettier 注重于 Formatting rules，我们可以使用 Prettier 对代码进行格式化。
- Airbnb 是前端最为流行的 JavaScript 规范。

此外，

- 如果您使用 Vue：eslint-plugin-vue
- 如果您使用 Nuxt：eslint-plugin-nuxt
- 如果您使用 React：eslint-plugin-react

同时，在你的编辑器（通常是 VS Code）中配置 ESLint 这样平时写代码的时候，编辑器就能帮你修正一些简单的格式错误，同时提醒你有哪些不符合 lint 规范的的地方，并在命令行中提示你错误。

最后，搭配 `husky + lint-staged`，💩 will never slip into your code base!

## FAQs

### extend 与 plugin 的区别

- `extend` 提供的是 ESLint 现有规则的一系列预设
- 而 `plugin` 则提供了除预设之外的自定义规则，当你在 ESLint 自带的规则里找不到合适的规则时，就可以使用插件来实现了。

### 如何通过环境变量来动态设置 rule

你可以通过环境变量来动态设置 rule：

```js
const isDev = process.env.NODE_ENV !== 'production'

module.exports = {
  rules: {
    'no-console': isDev ? 'warn' : 'error',
    'no-debugger': isDev ? 'warn' : 'error',
  },
}
```
