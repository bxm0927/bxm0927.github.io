---
title: Promise 对象
tags: [JavaScript, ES6]
categories: [JavaScript]
---

## Promise 对象简介

Promise 是异步编程的一种更优的解决方案，一定程度上解决了回调函数产生的“**回调地狱**”的问题。Promise 最早由社区提出和实现，ES6 将其写进了语言标准，统一了用法，原生提供了 `Promise` 对象。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

==Promise 允许将回调函数的嵌套改成**链式调用**，即用同步的方式去写异步代码，使异步代码看起来像是线性结构。==

![](https://ws1.sinaimg.cn/large/9823cde9gy1g4nth71v66j20kg0ckaad.jpg)

<!-- more -->

## Promise 对象的两个特点

1. 对象的状态不受外界影响。

Promise 对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和 rejected（已失败）。只有异步操作的结果可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是 Promise 这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。

Promise 对象的状态改变，只有两种可能：从 pending 变为 fulfilled 和从 pending 变为 rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对 Promise 对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

> 注意，我们一般说的 resolved，是指 Promise 对象的状态从 pending 变为 fulfilled，即异步操作成功。

## Promise 对象的缺点

Promise 也有一些缺点：

1. 首先，无法取消 Promise，一旦新建它就会立即执行，无法中途取消。
2. 其次，如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部。
3. 第三，当处于 pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。


## Promise 对象基本用法

ES6 规定，`Promise` 对象是一个构造函数，用来生成 `Promise` 实例。该构造函数接受一个函数作为参数，该函数的两个参数分别是 `resolve` 和 `reject`。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

- `resolve` 函数的作用是，将 Promise 对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；
- `reject` 函数的作用是，将 Promise 对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
- 如果调用 `resolve` 函数和 `reject` 函数时带有参数，那么它们的参数会被传递给回调函数。

```
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

Promise 实例生成以后，可以用 then 方法分别指定 resolved 状态和 rejected 状态的回调函数。then 方法可以接受两个回调函数作为参数。
- 第一个回调函数是 Promise 对象的状态变为 resolved 时调用，
- 第二个回调函数是 Promise 对象的状态变为 rejected 时调用。
- 其中，第二个函数是可选的，不一定要提供。
- 这两个函数都接受 Promise 对象传出的值作为参数。

```
promise.then(
  function(value) {
    // success
  },
  function(error) {
    // failure
  },
)
```

下面是一个 `Promise` 对象的简单例子。

```
// 一段时候后进行打印
function timeout(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms, 'done')
  })
}

timeout(3000).then(value => {
  console.log(value)
})

```


## Promise.prototype.then()

> Promise 实例具有 then、catch、finally 方法，也就是说，then、catch、finally 方法是定义在原型对象 Promise.prototype 上的。

then 方法的作用是为 Promise 实例**添加状态改变时的回调函数**。

then 方法可以接受两个回调函数作为参数。第一个回调函数是 Promise 对象的状态变为 resolved 时调用，第二个回调函数是 Promise 对象的状态变为 rejected 时调用（可选）。

then 方法返回的是一个新的 Promise 实例（注意，不是原来那个Promise实例）。因此可以采用链式写法，即 then 方法后面再调用另一个 then 方法。

```js
getJSON('/post/test.json')
  .then(post => getJSON(post.commentURL))
  .then(
    comments => console.log('resolved: ', comments),
    err => console.log('rejected: ', err)
  )
```

## Promise.prototype.catch()

Promise.prototype.catch 方法是 `.then(null, rejection)` 或 `.then(undefined, rejection)` 的别名，**用于指定发生错误时的回调函数**。

```js
promise
  .then(val => console.log('fulfilled:', val))
  .catch(err => console.log('rejected', err))

// 等同于
promise
  .then(val => console.log('fulfilled:', val))
  .then(null, err => console.log('rejected:', err))

```

Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个 catch 语句捕获。

最佳实践：一般来说，不要在 then 方法里面定义 `reject` 状态的回调函数（即 then 的第二个参数），总是使用 catch 方法。一般总是建议，Promise 对象后面要跟 catch 方法，这样可以处理 Promise 内部发生的错误。

```js
// bad
promise.then(
  function(data) {
    // success
  },
  function(err) {
    // error
  },
)

// good
promise
  .then(function(data) {
    // success
  })
  .catch(function(err) {
    // error
  })

```



## Promise.prototype.finally()

finally 方法用于指定不管 Promise 对象最后状态如何，**都会执行的操作**。该方法是 ES2018 引入标准的。

我们一般在 finally 方法中关闭服务和资源。

finally 方法的回调函数不接受任何参数，这意味着没有办法知道前面的 Promise 状态到底是 fulfilled 还是 rejected。这表明，finally 方法里面的操作，应该是与状态无关的，不依赖于 Promise 的执行结果。

```js
promise
  .then(result => {})
  .catch(error => {})
  .finally(() => {})
```

finally 本质上是 then 方法的特例。如果不使用 finally 方法，同样的语句需要为成功和失败两种情况各写一次。有了 finally 方法，则只需要写一次。


```
promise
  .finally(() => {
    // 都会执行的操作
  })

// 等同于
promise.then(
  result => {
    // 都会执行的操作
    return result
  },
  error => {
    // 都会执行的操作
    throw error
  },
)

// 等同于
promise
  .then(result => {
    // 都会执行的操作
    return result
  })
  .catch(error => {
    // 都会执行的操作
    throw error
  })

```

Promise.prototype.finally() 底层实现：

```
Promise.prototype.finally = function(callback) {
  let P = this.constructor
  return this.then(
    value => P.resolve(callback()).then(() => value),
    reason => P.resolve(callback()).then(() => throw reason),
  )
}

```

## Promise.all()

Promise.all 方法用于将多个 Promise 实例，包装成一个新的 Promise 实例（**并发请求**）。

```js
const p = Promise.all([p1, p2, p3]);
```

Promise.all 方法接受一个数组作为参数，p1、p2、p3 都是 Promise 实例。p 的状态由 p1、p2、p3 决定，分成两种情况。

1. 只有p1、p2、p3的状态都变成 fulfilled，p 的状态才会变成 fulfilled，此时 p1、p2、p3 的返回值组成一个数组，传递给 p 的回调函数。
2. 只要 p1、p2、p3 之中有一个被 rejected，p 的状态就变成 rejected，此时第一个被 reject 的实例的返回值，会传递给 p 的回调函数。

```js
function someAsyncThing(value) {
  return new Promise((resolve, reject) => {
    // 这里假如是一个异步操作
    value > 10 ? resolve(value) : reject(value)
  })
}

Promise.all([someAsyncThing(11), someAsyncThing(12)])
  .then(res => {
    console.log('sussess ' + res)
  })
  .catch(err => {
    console.log('error ' + err)
  })
// sussess [11,12]

Promise.all([someAsyncThing(11), someAsyncThing(10)])
  .then(res => {
    console.log('sussess ' + res)
  })
  .catch(err => {
    console.log('error ' + err)
  })
// error 10
```

## Promise.race()

> race：赛跑

Promise.race 方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

```js
const p = Promise.race([p1, p2, p3]);
```

上面代码中，只要 p1、p2、p3 之中有一个实例率先改变状态，p 的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给 p 的回调函数。


## Promise.resolve()

Promise.resolve 方法用于将现有对象转为 Promise 对象。

Promise.race 方法的参数与 Promise.all 方法一样，如果不是 Promise 实例，就会先调用 Promise.resolve 方法，将参数转为 Promise 实例，再进一步处理。

```
// 将 jQuery 生成的 deferred 对象，转为一个新的 Promise 对象
const jsPromise = Promise.resolve($.ajax('/whatever.json'));
```

```
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
```

## Promise.reject()

Promise.reject(reason) 方法也会返回一个新的 Promise 实例，该实例的状态为 rejected。

## Promise.try()

让同步函数同步执行，异步函数异步执行，并且让它们具有统一的 API。

```
const f = () => console.log('now');
Promise.try(f);
console.log('next');
// now
// next
```



## Promise 库

Bluebird：http://bluebirdjs.com/docs/api/promise.try.html

Q：https://github.com/kriskowal/q

when：https://github.com/cujojs/when




## 注意事项

1. Promise 新建后就会立即执行

```js
let promise = new Promise(function(resolve, reject) {
  console.log('Promise')
  resolve()
})

promise.then(function() {
  console.log('resolved.')
})

console.log('Hi!')

// Promise
// Hi!
// resolved
```

```js
setTimeout(function() {
  console.log(1)
}, 0)

new Promise(function executor(resolve) {
  console.log(2)
  for (var i = 0; i < 10000; i++) {
    i == 9999 && resolve()
  }
  console.log(3)
}).then(function() {
  console.log(4)
})

console.log(5)

// 2 3 5 4 1
```

2. 调用 resolve 或 reject 并不会终结 Promise 的参数函数的执行。

```js
new Promise((resolve, reject) => {
  resolve(1)
  console.log(2)
}).then(r => {
  console.log(r)
})
// 2
// 1
```

一般来说，调用 resolve 或 reject 以后，Promise 的使命就完成了，后继操作应该放到 then 方法里面，而不应该直接写在 resolve 或 reject 的后面。所以，最好在它们前面加上 return 语句，这样就不会有意外。

```
new Promise((resolve, reject) => {
  return resolve(1)
  // 后面的语句不会执行
  console.log(2)
})
```



3. Promise 的 `resolve` 函数的参数是另一个 Promise 实例。即一个 Promise p1 的结果是返回另一个 Promise p2，那么 p1 的状态由 p2 决定。

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error('fail')), 3000)
})

const p2 = new Promise((resolve, reject) => {
  setTimeout(() => resolve(p1), 1000)
})

p2
  .then(result => console.log(result))
  .catch(error => console.log(error))

// 表现：3s 后打印 "Error: fail"
```

4. Promise 内部的错误不会影响到 Promise 外部的代码，通俗的说法就是“Promise 会吃掉错误”。

```js
const someAsyncThing = function() {
  return new Promise(function(resolve, reject) {
    // 下面一行会报错，因为x没有声明
    resolve(x + 2)
  })
}

someAsyncThing().then(function() {
  console.log('everything is great')
})

setTimeout(() => {
  console.log(123)
}, 2000)
// Uncaught (in promise) ReferenceError: x is not defined
// 123

```

5. 如果 Promise 状态已经变成 resolved，再抛出错误是无效的，即不会被捕获。因为 Promise 的状态一旦改变，就永久保持该状态，不会再变了。

```js
const promise = new Promise(function(resolve, reject) {
  resolve('ok')
  throw new Error('test')
})
promise
  .then(value => console.log(value))
  .catch(error => console.log(error))
// ok

```
