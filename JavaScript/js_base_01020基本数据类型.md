---
tags: [javascript]
---

# JavaScript基本数据类型

## 数据类型

ECMAScript 标准定义了 8 种数据类型：

| 数据类型 | 说明 | 定义 |
| --- | --- | --- |
| boolean | 布尔值（Boolean） | 有2个值分别是：`true` 和 `false`. |
| number | 数字（Number），整数或浮点数 | 例如： `42` 或者 `3.14159` |
| bigint | 任意精度的整数 (BigInt) ，可以安全地存储和操作大整数，甚至可以超过数字的安全整数限制。 | 例如：`BigInt(Number.MAX_SAFE_INTEGER)` 或 `10n` |
| string | 字符串（String），字符串是一串表示文本值的字符序列 | `"Hello,JavaScript!"` |
| symbol | 代表（Symbol） ( 在 ECMAScript 6 中新添加的类型).。一种实例是唯一且不可改变的数据类型。 | 例如：`Symbol("Hello")` |
| object | 对象（Object） | 例如: `{foo: "foo"}` `["foo", "bar"]` |
| undefined | 一个没有被赋值的变量会有个默认值 `undefined` |  |
| null | 表示变量未指向任何对象 | Null 类型只有一个值： `null` |

## Boolean布尔值

## Number数字

### 判断无穷大/无穷小

```js
const a = 1 / 0
if (isFinite(a)) {
	// ⚠️ 不是无穷
} else {
	// 是无穷
}
```

## BigInt

## String字符串

## Object对象

## 数组 



##  `typeof`查看/判断数据类型

```javascript
console.log(typeof true) // boolean
console.log(typeof 3.14) // number
console.log(typeof 10n) // bigint
console.log(typeof "hello") // string
console.log(typeof Symbol("hello")) // symbol
console.log(typeof {foo: "foo"}) // object
console.log(typeof undefined) // undefined
console.log(typeof null) // object
```

`===`判断`null`类型：

```javascript
let a = null
console.log(a === null)
```

## 数据类型的转换