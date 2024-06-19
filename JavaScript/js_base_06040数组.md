---
tags: [javascript, array]
---

# 数组
## 定义`[]`

```javascript
const arr = ['a', 'b', 'c']
```

## 访问`[]`

```javascript
const arr = ['a', 'b', 'c']
console.log(arr[0])
```

## 长度`length`

```javascript
const arr = ['a', 'b', 'c']
console.log("数组长度是", arr.length)
```

## 添加元素

### push

`push`末端添加一个或多个元素

```javascript  
// push 在数组的末端添加一个或多个元素  
// unshift在数组的第一个位置添加元素
const arr = ['a', 'b', 'c']
arr.push('d', 'e') 
console.log(arr) // ["a", "b", "c", "d", "e"]
```

### unshift

`unshift`在数组的第一个位置添加元素

```js
// unshift在数组的第一个位置添加元素
const arr = ['a', 'b', 'c']
arr.unshift('d', 'e')
console.log(arr) // ["d", "e", "a", "b", "c"]
```

## 修改元素`[]`

```javascript
const arr = ['a', 'b', 'c']
arr[2] = 'd'
```

## 移除元素

### pop

`pop`方法用于删除数组的最后一个元素，并返回该元素。

```javascript
// pop方法用于删除数组的最后一个元素，并返回该元素。
// 空数组pop返回undefined。
const arr = ['a', 'b', 'c']
arr.pop()
console.log(arr) // ["a", "b"]  
```

### shift

`shift()`方法用于删除数组的**第一个**元素，并返回该元素。**空数组**`shift`返回`undefined`。

```js
// shift()方法用于删除数组的第一个元素，并返回该元素。
// 空数组shift返回undefined。
const arr = ['a', 'b', 'c']
arr.shift()
console.log(arr) // ["b", "c"]  
```

### `splice`

通过索引删除元素

```js
// 通过索引删除某元素
const removedItem = arr.splice(1, 1)
console.log(arr)
```

## 遍历

```javascript
const arr = ['a', 'b', 'c']
arr.foo = 'foo'
  
// for循环
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i])
}
  
// for of循环
for (const value of arr) {
    console.log(value)
}
  
// forEach
arr.forEach((item, index) => {
    console.log("元素值 ", item, "  索引 ", index)
})
  
// for in循环
// 但是，for...in不仅会遍历数组所有的数字键，还会遍历非数字键。
for (const key in arr) {
    console.log(arr[key])
}
```

## 数组转字符串

```javascript
const arr = ['a', 'b', 'c']
  
// toString方法也是对象的通用方法，数组的toString方法返回数组的字符串形式。
const str1 = arr.toString()
console.log(str1) // a,b,c
  
// join()方法以指定参数作为分隔符，将所有数组成员连接为一个字符串返回。
// 如果不提供参数，默认用逗号分隔。
const str2 = arr.join('-')
console.log(str2) // a-b-c
```

## 判断是否存在否元素

```javascript
const arr = ['a', 'b', 'c']
  
let isExist = false
  
// includes() 方法用来判断一个数组是否包含一个指定的值，如果包含则返回 true，否则返回false。
isExist = arr.includes('b')
console.log(isExist ? '包含' : '不包含')
  
// indexOf()方法返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。
isExist = arr.indexOf('b') !== -1
console.log(isExist ? '包含' : '不包含')
```

## 数组合并concat

```javascript
const arr = ['a', 'b']
  
const newArr = arr.concat(['c', 'd'], ['e', 'f'])
console.log(newArr) // ["a", "b", "c", "d", "e", "f"]
```

## 获取某元素索引indexOf

```javascript
const arr = ['a', 'b']
  
const index = arr.indexOf('b')
console.log(index) // 1
```

## 数组截取

### slice

`ArrayObject.slice(start?, end?)`方法有两个参数

- `start`: 开始位置索引。默认为0。负数表示从后面计算。-1表示最后一个元素。
- `end`：结束位置索引。如果没有指定，表示截取到数组最后。负数表示从后面计算。
- 如果`start`**大于**`end`，返回**空**数组。

```js
const arr = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']

const arr2 = arr.slice(1, 4)
console.log(arr2) // [ 'b', 'c', 'd' ]

const arr3 = arr.slice(-4, -1)
console.log(arr3) // [ 'g', 'h', 'i' ]

const arr4 = arr.slice(-1, 4)
console.log(arr4) // []

const arr5 = arr.slice(5)
console.log(arr5) // [ 'f', 'g', 'h', 'i', 'j' ]

const arr6 = arr.slice() 
console.log(arr6) // ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
```

## map

```javascript
const arr = [1, 2, 3]
  
// map() 方法创建一个新数组，其结果是该数组中的每个元素是调用一次提供的函数后的返回值。
const newArr = arr.map(item => (item * item))
console.log(newArr) // [1, 4, 9]
```

## flatmap

```javascript
const arr = [1, 2, 3]
  
// flatMap() 方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。
const newArr = arr.flatMap(item => (new Array(item).fill(item)))
console.log(newArr) // [1, 2, 2, 3, 3, 3]
```

## reduce

```javascript
const arr = [1, 2, 3]
  
// reduce() 方法对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值。
const result = arr.reduce((accumulator, currentValue) => accumulator + currentValue, 0)
console.log(result) // 6
```

## filter

```javascript
const arr = [1, 2, 3, 4]
  
// filter() 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。 
const result = arr.filter(item => item % 2 == 0)
console.log(result) // [2, 4]
```

## splice

```javascript

```

## 查找`find`

```javascript
const arr = [1, 2, 3, 4]
  
// filter() 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。 
const result = arr.find(item => item % 2 == 0)
console.log(result) // 2
  
// findIndex()方法返回数组中满足提供的测试函数的第一个元素的索引。若没有找到对应元素则返回-1。
const result2 = arr.findIndex(item => item % 2 == 0)
console.log(result2)  // 1
```

## 反转`reverse`

```javascript
const arr = [1, 2, 3, 4]
  
// 反转数组
const result = arr.reverse()
console.log(result) // [4, 3, 2, 1]
```

## 快速生成数组

```javascript
// 快速生成数组序列
let arr1 = Array.from(new Array(4).keys()) // [0, 1, 2, 3]

// 快速生成数组
let arr2 = Array.from({length:4}, (item, index)=> index + 1) // [1, 2, 3, 4]

// fill
new Array(4).fill('').forEach((item, index) => console.log(index)) // 0, 1, 2, 3
```
