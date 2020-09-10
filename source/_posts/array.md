---
title: JavaScript 数组知识点梳理
date: 2019/5/19 23:12:29
updated: 2019-07-01 14:29:50
tags: [HTML, CSS, JS]
categories: [JS]
---

## Array 简介

在 JavaScript 中，Array 是标准的内置对象，其他常见的内置对象有 Object、Number、String、Boolean、Function、RegExp、Date、Math，还有 ES6 新增的 Map、Set 等。

Array 对象用于在单个的变量中存储多个值，在 JavaScript 中，这些值的数据类型可以不同，甚至可以是其他数组或一个对象。

> JavaScript 内置对象相当于 Java 的类库（Java APIs）。
>
> 在 Java 中，数组中的每个元素必须是相同的数据类型。

## 创建数组的几种方式

### 构造器

```javascript
const arr = new Array()
const arr = new Array(size)
const arr = new Array(element0, element1, ..., elementn)
```

如果调用构造函数时 没有使用参数，那么返回的数组为空，即 length 为 0。

如果调用构造函数时 只传递了一个数字参数，该构造函数将返回具有指定个数、元素为 `undefined` 的数组。

如果调用构造函数时 传递了多个参数，那么该构造函数将用这些参数来初始化数组。

### 字面量

字面量又叫直接量（literal syntax）

```javascript
const arr = []
const arr = [element0, element1, ..., elementn]
```

在大多数情况下，推荐使用字面量赋值，数组用 `[]`，对象用 `{}`，正则用 `/.../` 等。

```javascript
// bad
const items = new Array()

// good
const items = []
```

## 多维数组

在 JavaScript 中，多维数组本质上还是一维数组，因为数组的元素也可以是一个数组。

实例：

```javascript
const multiArr = [
  [1, 2, 3],
  [2, 3, 4],
  [3, 4, 5],
]

// 如何取到中间那个数？
```

## 数组属性

### length

设置或返回数组中元素的数目。

设置 `length` 属性可改变数组的大小，需要注意的是：

- 如果设置的值比其当前值小，数组将被截断，其尾部的元素将丢失。
- 如果设置的值比它的当前值大，数组将增大，新的元素被添加到数组的尾部，它们的值为 `undefined`。

> 在 Java 中，当数组的下标超过其长度（`length - 1`）时，会产生“数组越界错误（ArrayIndexOutOfBoundsException）”。

示例：

```javascript
const myArr = [1, 2, , 3, 4, 5]
myArr.length = 10
// [1, 2, ,3, 4, 5, undefined, undefined, undefined, undefined, undefined]
```

`length` 妙用：

- `arr[arr.length - 1]` 总是指向数组最后一个元素
- `arr[arr.length]` 总是指向数组最后一个元素的下一个

```javascript
// 获取数组第一个元素
function first(arr) {
  return arr[0]
}

// 获取数组最后一个元素
function last(arr) {
  return arr[arr.length - 1]
}

// 向数组追加新元素，相当于 `arr.push()`
function append(arr, el) {
  return (arr[arr.length] = el)
}

// 使用 `arr.length -= 1` 删除数组最后一个元素，相当于 `arr.pop()`
function remove(arr, el) {
  return (arr.length -= 1)
}

// 清空数组
function empty(arr) {
  return (arr.length = 0)
}
```

### prototype

原型，使您有能力向对象添加属性和方法。

`prototype` 妙用：

```javascript
// 如果 Array 本身不提供 `first()` 方法，添加一个返回数组的第一个元素的新方法。
if (!Array.prototype.first) {
  Array.prototype.first = function () {
    return this[0]
  }
}

// 如果 Array 本身不提供 `last()` 方法，添加一个返回数组的最后一个元素的新方法。
if (!Array.prototype.last) {
  Array.prototype.last = function () {
    return this[this.length - 1]
  }
}

// 如果 Array 本身不提供 `append(el)` 方法，添加一个向数组追加新元素的新方法。
if (!Array.prototype.append) {
  Array.prototype.append = function (el) {
    return (this[this.length] = el)
  }
}

// 如果 Array 本身不提供 `remove()` 方法，添加一个删除数组最后一个元素的新方法。
if (!Array.prototype.remove) {
  Array.prototype.remove = function (el) {
    return (this.length -= 1)
  }
}
```

### constructor

返回对创建此对象的数组函数的引用。

示例：

```javascript
const arr = [1, 2, 3]
arr.constructor === Array // true
```

## 数组实例常用方法

- 会改变自身的方法：push()、pop()、shift()、unshift()、reverse()、sort()
- 不会改变自身的方法：concat()、join()、slice()

### push()

JavaScript Array 提供了一些操作栈/队列的方法：`push()、pop()、shift()、unshift()`

![image.png](https://wx1.sinaimg.cn/large/9823cde9gy1g6vlxmghsej20al052jrc.jpg)

入栈。向数组的末尾添加一个或更多元素，并返回新的长度。

语法：

```javascript
arrayObject.push(newelement1, newElement2, ..., newElementN)
```

实例：

```javascript
let arr = ['Google', 'John', 'Thomas']
arr.push('xiaoming', 'xiaoqiang') // 5
arr // ["Google", "John", "Thomas", "xiaoming", "xiaoqiang"]
```

### pop()

出栈。删除并返回数组的最后一个元素。

注意：如果数组已经为空，则 `pop()` 不改变数组，并返回 `undefined` 值。

语法：

```javascript
arrayObject.pop()
```

示例：

```javascript
let arr = ['Google', 'John', 'Thomas']
arr.pop() // "Thomas"
arr // ["Google", "John"]
```

### shift()

出队。删除并返回数组的第一个元素。

注意：如果数组已经为空，则 `shift()` 不改变数组，并返回 `undefined` 值。

语法：

```javascript
arrayObject.shift()
```

示例：

```javascript
let arr = ['Google', 'John', 'Thomas']
arr.shift() // "Google"
arr // ["John", "Thomas"]
```

### unshift()

向数组的开头添加一个或更多元素，并返回新数组的长度。

> `shift()` 和 `unshift()` 并不是严格的队列操作（FIFO），严格的队列操作只能在线性表的一端进行删除操作，在另一端进行插入操作。

语法：

```javascript
arrayObject.unshift(newelement1, newElement2, ..., newElementN)
```

示例：

```javascript
let arr = ['Google', 'John', 'Thomas']
arr.unshift('xiaoming', 'xiaoqiang') // 5
arr // ["xiaoming", "xiaoqiang", "Google", "John", "Thomas"]
```

### concat()

连接两个或更多的数组，并返回结果。

语法：

```javascript
arrayObject.concat(array1, array2, ..., arrayN)
```

示例：

```javascript
const arr1 = ['Google', 'John', 'Thomas']
const arr2 = ['James', 'Adrew', 'Martin']
const arr3 = ['xiaoming']
arr1.concat(arr2, arr3)
// ["Google", "John", "Thomas", "James", "Adrew", "Martin", "xiaoming"]
```

### slice(start, end)

从某个已有的数组返回选定的元素。

语法：

```javascript
arrayObject.slice(start, end) // [start, end)
```

| 参数  | 描述                                                                                                                                                                                          |
| :---- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| start | 必需。规定从何处开始选取。如果是负数，那么它规定从数组尾部开始算起的位置。也就是说，-1 指最后一个元素，-2 指倒数第二个元素，以此类推。                                                        |
| end   | 可选。规定从何处结束选取。该参数是数组片断结束处的数组下标。如果没有指定该参数，那么切分的数组包含从 start 到数组结束的所有元素。如果这个参数是负数，那么它规定的是从数组尾部开始算起的元素。 |

示例：

```javascript
const arr = ['Google', 'John', 'Thomas', 'xiaoming']
arr.slice(0, 1) // ？
arr.slice(1, 3) // ？
arr.slice(2) // ？
arr.slice(-1) // ？
```

```javascript
// 利用 `slice()` 实现数组拷贝
function arrCopy(arr) {
  return arr.slice(0)
}
```

### `splice(index, howmany[, item1, ..., itemN])`

向数组中添加元素；或从数组中删除元素，然后返回被删除的项目。

Tips：`slice()` 方法并不会修改数组，而是返回一个新数组。如果想删除数组中的一段元素，应该使用方法 `splice()`。

语法：

```javascript
arrayObject.splice(index, howmany, item1, ..., itemN)
```

| 参数              | 描述                                                                  |
| :---------------- | :-------------------------------------------------------------------- |
| index             | 必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。 |
| howmany           | 必需。要删除的项目数量。如果设置为 0，则不会删除项目。                |
| item1, ..., itemN | 可选。向数组添加的新项目。                                            |

示例：

```javascript
let arr = ['Google', 'John', 'Thomas', 'xiaoming']

arr.splice(2, 0, 'tim.wang') // ？
arr // ？

arr.splice(3, 1, 'dongmei') // ？
arr // ？
```

```js
/**
 * Removes an element from an array, and return new arr
 * @param {Array} arr
 * @param {*} element
 */
function remove(arr, el) {
  return arr.splice(arr.indexOf(el), 1)
}
```

### join(separator)

把数组的所有元素放入一个字符串，元素通过指定的分隔符进行分隔。

注意：如果省略分隔符参数，则默认使用英文逗号作为分隔符。

语法：

```javascript
arrayObject.join(separator)
```

示例：

```javascript
const arr = ['Google', 'John', 'Thomas']
arr.join() // "Google,John,Thomas"
arr.join(' - ') // "Google - John - Thomas"
```

### toString()

把数组转换为字符串，并返回结果。

返回值与没有参数的 `join()` 方法返回的字符串相同。

```javascript
const arr = ['George', 'John', 'jun', 'dongmei', 'xiaoming']
arr.toString() // "George,John,jun,dongmei,xiaoming"
```

### reverse()

颠倒数组中元素的顺序。

注意：该方法会改变原来的数组，而不会创建新的数组。

语法：

```javascript
arrayObject.reverse()
```

示例：

```javascript
let arr = ['Google', 'John', 'Thomas']
arr.reverse() // ["Thomas", "John", "Google"]
```

### sort()

对数组的元素进行排序。

如果调用该方法时没有使用参数，将按字母顺序对数组中的元素进行排序，说得更精确点，是按照字符编码的顺序进行排序。如果想按照其他标准进行排序，就需要提供比较函数。

> 类似 Java 的比较器接口 `java.lang.Comparable`、`java.util.Comparator`

注意：`sort()` 方法在原数组上进行排序，不生成新数组。

语法：

```js
arrayObject.sort(sortby)
```

示例：

```js
let arr = [1, 3, 2]
arr.sort() // [1, 2, 3]
arr // [1, 2, 3]
```

```js
/**
 * Array shuffle
 * @param {Array} arr
 */
function shuffle(arr) {
  return arr.sort(() => Math.random() - 0.5)
}
```

### `fill(value[, start, end])`

使用 `value` 填充数组，填充范围是 `[start, end)`

```
new Array(10).fill(0, 2, 4)
// [empty × 2, 0, 0, empty × 6]
```

### `copyWithin(target[, start, end])`

在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员）

```
[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]
```

### `flat`、`flatMap`

数组平铺

## 数组实例查询方法

### `includes`

### `indexOf`、`lastIndexOf`

indexOf() 返回一个指定的字符串值第一次出现的位置索引

lastIndexOf() 返回一个指定的字符串值最后出现的位置索引

```
const arr = [1, 2, 2, 3, 3, 4]
arr.indexOf(2) // 1
arr.lastIndexOf(2) // 2
```

### `find`、`findIndex`

find：返回第一个符合条件的子项

findIndex：返回第一个符合条件的子项的索引

```
const arr = [1, 2, 3]
arr.find(i => i > 1) // 2
arr.findIndex(i => i > 1) // 1
```

## 数组迭代方法

### for

最最最基本的数组遍历方法。

使用 `break` 语句停止遍历

```js
for (let i = 0; i < arr.length; i++) {
  const element = arr[i]
  // ...
}
```

### forEach

for 写法比较麻烦，因此数组提供更简单便捷的 `forEach()` 方法。

如何中断 forEach 遍历？

- throw + try catch
- 用 some() 改写
- 用 for 改写

```js
let arr = [10, 20, 30, 25, 15]
arr.forEach((item, index) => {
  console.log(item + 10) // 20, 30, 40, 35, 25
})

arr // [10, 20, 30, 25, 15]
```

### for...in

遍历数组的索引，包括原型链上的键。

```js
const arr = [10, 20, 30, 25, 15]
for (const index in arr) {
  console.log(index, arr[index])
}
// 0 10
// 1 20
// 2 30
// 3 25
// 4 15
```

使用 `for...in` 循环遍历数组有以下几个缺点：

- 数组的键名是数字，但是 for...in 循环是以字符串作为键名“0”、“1”、“2”等等。
- for...in 循环不仅遍历数字键名，还会遍历手动添加的其他键，甚至包括原型链上的键。
- 某些情况下，for...in 循环会以任意顺序遍历键名。

总之，`for...in` 循环主要是为遍历对象而设计的，不适用于遍历数组。

```js
for (const key in object) {
  if (object.hasOwnProperty(key)) {
    const element = object[key]
    // ...
  }
}
```

### for...of

`for...of` 是 ES6 新增的遍历方法，用来遍历具有 Iterator 接口（类数组）的数据结构。

使用 `break` 语句停止遍历

```js
const arr = [10, 20, 30, 25, 15]
for (const item of arr) {
  console.log(item) // 10, 20, 30, 25, 15
}
```

### filter

筛选出数组中符合条件的元素，组成新的数组。所以结果的长度会小于等于原始数组。

```js
const arr = [10, 20, 30, 25, 15]
const newArr = arr.filter((item, index) => item < 20)

arr // [10, 20, 30, 25, 15]
newArr // [10, 15]
```

### map

更新数组中的每个值，并返回一个相同长度的新数组，而不改变原始数组。

```js
const arr = [10, 20, 30, 25, 15]
const newArr = arr.map((item, index) => item * 2)

arr // [10, 20, 30, 25, 15]
newArr // [20, 40, 60, 50, 30]
```

### `reduce(callback[, initialValue])`

`reduce()` 方法对数组中的每个元素执行一个由您提供的 reducer 函数，将其结果汇总为单个返回值（即归约）。

在每次遍历中的 `callback(accumulator, currentValue[, index[, array]])` 参数为：累加器，当前项，索引和数组本身且应该返回累加器。

reduce 是这样工作的：你传入一个函数，遍历数组的每一个元素时都会调用你传入的函数。你的函数调用时会接收两个参数：上一次迭代的结果，和当前数组元素。它结合当前元素和之前的 total 结果然后返回新的 total 值。

示例：

```
const letters = ['r', 'e', 'd', 'u', 'x']

// reduce 接收两个参数:
// - 一个用来 reduce 的函数 (也称为 "reducer")
// - 一个计算结果的初始值
// 注意：这里的初始值是一个空字符串
const word = letters.reduce((acc, item) => acc + item, '')
console.log(word) // "redux"
```

```
/**
 * 数组拼接
 * 你也可以使用：`arr1.concat(arr2)`、`[...arr1, ...arr2]`
 * @param  {Array} arg 支持多数组
 */
export const concat = (...arg) => arg.reduce((acc, cur) => [...acc, ...cur], [])

/**
 * 数组求和
 * @param {Array} arr
 */
export const sum = (arr) => arr.reduce((acc, item) => acc + item, 0)
```

==Redux 状态管理库的原理与 reduce 函数有异曲同工之妙：==

```
// array.reduce
(acc, item) => nextAcc

// Redux reducers
(state, action) => newState
```

### `reduceRight(callback[, initialValue])`

`reduceRight()` 方法与 `reduce()` 类似，不同的是 `reduceRight()` 从右至左遍历。

```
const a = ['1', '2', '3', '4', '5']
const left = a.reduce((prev, cur) => prev + cur)
const right = a.reduceRight((prev, cur) => prev + cur)

console.log(left)  // "12345"
console.log(right) // "54321"
```

### every

检测数组中的每一项是否符合条件，全部满足才为 `true`

```js
const arr = [1, 2, 3, 4, 5]
const result = arr.every((item, index) => item > 3)

result // false
```

### some

检测数组中是否有某项符合条件，全不满足才为 `false`

```js
const arr = [1, 2, 3, 4, 5]
const result = arr.some((item, index) => item > 3)

result // true
```
