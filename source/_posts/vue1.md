---
title: 9102 年了，还没掌握 Vue？
date: 2020/6/1 23:10:10
updated: 2020-06-12 17:10:50
tags: [HTML, CSS, JS, Vue]
categories: [Vue]
---

![image](https://note.youdao.com/yws/res/126789/88C2D309D1E949AB8C44B8DDCEA0304D)

## 三大框架之前（Long Long Ago）

近年来，随着老式浏览器逐渐被淘汰，并且移动端需求增加，前端所需要的交互越来越多，功能也越来越复杂，架构从以后端为主的 MVC 正在向以前端为主的 MVVM 架构迁移，Vue、React、Angular 在此大环境下孕育而生。

Angular 就不谈了，由于框架本身承载的功能过多，上手太难，导致这个框架没有其他流行。在世界范围内，React 是最流行的，因为它是由 Facebook 这样的大公司所维护。但是在中国，React 和 Vue 旗鼓相当，因为 Vue 的作者就是中国人，并且拥有良好的文档和简单易上手的 API 操作。

- https://github.com/vuejs/vue
- https://github.com/facebook/react
- https://github.com/angular/angular

![image](https://note.youdao.com/yws/res/126798/9675A244C5234C69B6C2D5E5C06034ED)

## Vue 简介（Introduction）

- https://vuejs.org/
- https://cn.vuejs.org/
- https://github.com/vuejs/vue

Vue.js 是一个开源的、渐进式的、用于构建用户界面的 JavaScript 框架。

Vue 的目标是通过尽可能简单的 API 实现==响应的数据绑定和组系统件==，通过操作数据的方式来操作 DOM，进而让 DOM 操作绝迹。

Vue 的核心库只关注视图层，但是 Vue 拥有丰富的生态资源，你可以根据需要在项目中渐进式地引入它们。

兼容性： Vue 不支持 IE8 及以下版本。

![image](https://note.youdao.com/yws/res/113778/0E277F7E8FD744EEBA881528946D88A4)

## Vue 特性（Features）

1. 声明式渲染：区别于以 jQuery 为代表的命令式渲染。Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统。

![image](http://v1-cn.vuejs.org/images/mvvm.png)

2. 组件系统：用解耦、可复用的组件来构造界面，每个组件都包含属于自己的 HTML/CSS/JS。

![image](https://note.youdao.com/yws/res/126876/AC9AFF6D37CB4F9F9371680303ADC2E6)

## Vue 生态（Ecosystem）

https://github.com/vuejs/awesome-vue

Vue 有着优良的生态系统，比如你需要一个 UI 框架来快速搭建一个管理系统，你可以选择 element-ui、bootstrap-vue 等优秀的组件库；你需要使用路由，你可以使用 vue-router；你需要服务端渲染，你可以使用 Nuxt.js。

甚至，你需要构建桌面应用，你可以使用 electron-vue；你需要构建移动端跨平台应用，你可以使用阿里的 Weex；你需要构建微信小程序，你可以使用美团的 mpvue，等等等等。

![image](https://note.youdao.com/yws/res/123105/60465E2463D54D278305977A3A271A24)

## Vue 安装（Installation）

> 最新稳定版本：2.6.10

1. 直接用 `<script>` 引入

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
```

2. 在用 Vue 构建大型应用时推荐使用 NPM 安装

```bash
# 最新稳定版
$ npm install vue
```

## Vue 生命周期（Life Cycle）

每个 Vue 实例在被创建时都要经过一系列的初始化过程，在这个过程中会运行一些叫做==生命周期钩子==的函数，这给了用户在不同阶段添加自己的代码的机会。

Vue 生命周期的 8 个阶段：

1. 创建前、创建后（beforeCreate、created）
2. 载入前、载入后（beforeMount、mounted）
3. 更新前、更新后（beforeUpdate、updated）
4. 销毁前、销毁后（beforeDestroy、destroyed）

生命周期钩子函数使用技巧：

- 常用的生命周期钩子：created、mounted
- 在 created 阶段，DOM 还没渲染完成，可以先进行一些后端数据请求操作
- 在 mounted 阶段，由于组件 DOM 已经渲染完成，所以可以执行依赖于 DOM 的操作。
- 在 destroyed 阶段：移除事件监听器和定时器，避免内存泄露
- 第一次页面加载会触发哪几个钩子：会触发 beforeCreate、created、beforeMount、mounted

![生命周期图示](https://note.youdao.com/yws/res/117180/BA2D940981F842C9B1AF2E2EC1B7EF3A)

## Vue 常用 API（API）

### 指令

指令是 Vue 提供的特殊属性，带有前缀 `v-`，它们会在渲染的 DOM 上应用特殊的响应式行为。

- v-text：绑定元素文本内容

`v-text` 等价于文本插值，用于绑定元素文本内容。

```html
<span v-text="msg"></span> <span>{{ msg }}</span>
```

- v-html：绑定元素 HTML 内容

双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 v-html 指令：

```html
data() { return { message: '
<h1>hello</h1>
' } }

<p>Using mustaches: {{ message }}</p>
<p>Using v-html directive: <span v-html="message"></span></p>
```

- v-bind：绑定元素属性

用来绑定 DOM 属性，`v-bind:xxx` 可以简写为`:xxx`

```
<a v-bind:href="url">...</a>

<div v-bind:id="'list-' + id"></div>
```

- v-show：元素显示/隐藏

v-if 和 v-show 都可以根据条件展示元素，不同的是带有 v-show 的元素始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS 属性 display，即`style="display: none"`。

v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做。直到条件第一次变为真时，才会开始渲染条件块。相比之下，v-show 就简单得多：不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS display 进行切换。

```html
<h1 v-if="ok">Hello!</h1>
<h1 v-show="ok">Hello!</h1>
```

使用说明：v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

- v-if、v-else、v-else-if：条件渲染

v-if、v-for 用来绑定 DOM 结构。v-if 会根据条件展示元素。

- v-for：列表渲染

v-for 指令用来渲染一个项目列表，它可以绑定一个数组、对象、整数或字符串数据：

参数：

1. 数组渲染时，`v-for` 支持一个可选的第二个参数为当前项的索引
2. 对象渲染时，`v-for` 支持一个可选的第二个参数为当前项的键名，还支持一个可选的第三个参数为索引

```
<li v-for="(todo, index) in todos"></li>
<li v-for="(value, key, index) in nameObject"></li>
```

- v-model：表单绑定

v-model 指令用来绑定表单输入，如 `<input>`、`<textarea>` 及 `<select>`

- v-on：事件监听器

v-on 指令用来监听 DOM 事件，并在触发时运行一些 JavaScript 代码。可以简写为`@`。

v-on 可以监听多个事件：

```html
<a style="cursor:default" v-on="{ click: dosomething, mouseleave: mouseleave }">dosomething</a>
```

### 实例选项

每个 Vue 应用都是通过用 `new Vue()` 函数创建一个新的 Vue 实例开始的。当创建一个 Vue 实例时，你可以传入一些选项，使用 Vue 就是使用这些选项来创建你想要的行为。（vm：ViewModel）

```
const vm = new Vue({
  // 选项
})
```

- el 挂载点

el 是 Vue 根实例特有的选项，是 Vue 的挂载点，需要绑定到对应元素。

```html
<div id="app">
  <h1>{{ msg }}</h1>
</div>

<script>
  const vm = new Vue({
    el: '#app',
    data() {
      return {
        msg: 'Hello App!',
      }
    },
  })
</script>
```

- data 绑定数据

每个 Vue 实例都会代理其 data 对象里所有的属性，只有被代理的属性是响应的，也就是说属性值的任何改变都会触发视图的重新渲染。页面渲染时，data 对象中的数据会参与渲染。当这些数据改变时，视图会进行重渲染。

> [Vue warn]: The "data" option should be a function that returns a per-instance value in component definitions.

一个组件的 data 选项必须是一个函数，而不是对象。这样，当你复用组件时，每个组件实例可以维护一份被返回对象的独立的拷贝，不至于造成数据污染。

值得注意的是只有当实例被创建时 data 中存在的属性才是响应式的。也就是说如果你添加一个新的属性，那么对该属性的改动将不会触发任何视图的更新。如果你知道你会在晚些时候需要一个属性，但是一开始它为空或不存在，那么你仅需要设置一些初始值。比如：

```js
data () {
  return {
    text: '',
    count: 0,
    hideCompletedTodos: false,
    todos: [],
    error: null
  }
}
```

- methods 方法

Vue 实例的方法。

```js
const vm = new Vue({
  data() {
    return { a: 1 },
  },
  methods: {
    plus() {
      this.a++
    }
  }
})

vm.plus()
vm.a // 2
```

- computed 计算属性

需求：字符串反转，你可能会这样写：

```html
<div id="example">
  <p>Original message: {{ message }}</p>
  <p>reversed message: {{ message.split('').reverse().join('') }}</p>
</div>
```

虽然可以在插值中进行运算，但是在模板中放入太多的运算逻辑会让模板过重且难以维护。对于复杂的逻辑运算，且可能多次使用的情况下，你应当使用计算属性。

```
<div id="example">
  <p>Original message: {{ message }}</p>
  <p>reversed message: {{ reversedMessage }}</p>
</div>

computed: {
  reversedMessage() {
    return this.message.split('').reverse().join('')
  }
}
```

计算属性有如下特点：

1. 使得数据处理结构清晰
2. 计算属性会把计算结果进行缓存，并作为实例的一个属性存在
3. 依赖于数据，数据更新，处理结果自动更新
4. 常用的是 get 方法获取数据，也可以使用 set 方法设置数据
5. 相较于 methods，不管依赖的数据变不变，methods 都会重新计算，但是依赖数据不变的时候 computed 从缓存中获取，不会重新计算。

computed 与 methods 的区别：

1. ==计算属性是基于它们的依赖进行缓存的==。只在相关依赖发生改变时它们才会重新求值。这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。减小了计算开销。
2. 计算属性定义的函数作为实例的一个属性存在，而 `methods` 定义的函数作为实例的一个方法存在

> 我们为什么需要缓存？假设我们有一个性能开销比较大的的计算属性 A ，它需要遍历一个极大的数组和做大量的计算。然后我们可能有其他的计算属性依赖于 A 。如果没有缓存，我们将不可避免的多次执行 A 的 getter！

- filter 过滤器

需求：首字母大写，你可能会这样写：

```
<div id="example">
  <p>Original message: {{ message }}</p>
  <p>capitalize message: {{ message.toString().charAt(0).toUpperCase() + message.slice(1) }}</p>
</div>
```

虽然可以在文本插值和 v-bind 表达式中使用 JavaScript 表达式，但是在文本插值中进行大量的逻辑处理显然不太合适。这时可以使用过滤器 filter 来进行逻辑过滤处理。

过滤器设计目的就是用于简单的文本转换。

若要实现更复杂的数据变换，你应该使用计算属性。

```html
<!-- in mustaches -->
{{ message | capitalize }}

<!-- in v-bind -->
<div v-bind:id="rawId | formatId"></div>
```

```js
filters: {
    // 过滤器接收表达式的值 (之前的操作链的结果) 作为第一个参数
    capitalize(value = '') {
        return value.toString().charAt(0).toUpperCase() + value.slice(1) // 首字母大写
    }
}
```

- watch 侦听器

监听某个属性的变化，一旦该属性发生改变，相应操作就会执行。

这个和 RDBMS 的触发器（Trigger）类似。触发器是一种在数据库事件（插入，更新，删除行等情况）时发生时隐式自动运行的程序块。

```sql
CREATE TRIGGER reset_age before insert on student for each row
begin
  if NEW.age < 0 then
    set NEW.age = 0;
  end if;
end;
```

使用 watch 就是这样：

```js
watch: {
  age(value) {
    if (value < 0) {
      this.age = 0
    }
  }
}
```

- mixins 混入

将可复用的组件选项进行封装，使用时注意合并策略。

```js
// 定义一个混入对象
const myMixin = {
  mounted() {
    this.printHello()
  },
  methods: {
    printHello() {
      console.log('hello from mixin!')
    },
  },
}

// 使用
mixins: [myMixin]
```

## Vue 高级（Depth）

### 插件 Plugins

> awesome-vue 集合了来自社区贡献的数以千计的插件和库。

通过全局方法 `Vue.use()` 使用插件。它需要在你调用 `new Vue()` 启动应用之前完成：

```js
import Vue from 'vue'
import VueLazyload from 'vue-lazyload'

Vue.use(VueLazyload)

new Vue({
  // ... options
})
```

当然，你也可以开发 Vue 插件

### 服务端渲染 SSR？

我们为什么要使用 SSR？

- 更好的 SEO，让搜索引擎更好地收录页面
- 更快的内容到达时间 (time-to-content)

Vue 如何支持 SSR？

- Vue SSR：https://ssr.vuejs.org
- Nuxt.js：https://zh.nuxtjs.org
