---
tags: [javascript, object]
---

# 对象
## 定义

```javascript
const obj = {
  foo: 'Hello',
  bar: 'World',

  sayHello: function () {
    console.log(`${this.foo} ${this.bar}!`)
  }
}
```

## 属性的读取

```javascript
const obj = {
  foo: 'Hello',
  bar: 'World',
}

// 点运算符
console.log(obj.foo)
// 方括号运算符
console.log(obj['bar'])
```

## 属性的赋值

```javascript
const obj = {}

// 点运算符
obj.foo = 'Hello'
// 方括号运算符
obj['bar'] = 'World'
```

## 属性的查看

```javascript
const obj = { 
  foo: 'Hello', 
  bar : 'World'
}

// 查看一个对象本身的所有属性
console.log(Object.keys(obj))
```

## 属性的删除

```javascript
const obj = { 
  foo: 'Hello', 
  bar : 'World'
}
  
// delete命令用于删除对象的属性，删除成功后返回true。
delete obj.foo
  
console.log(Object.keys(obj))
```

## 判断是否存在某属性

```javascript
const obj = { 
  foo: 'Hello', 
  bar : 'World'
}
  
// in运算符
// in运算符用于检查对象是否包含某个属性，如果包含就返回true，否则返回false。
// in运算符不能识别哪些属性是对象自身的，哪些属性是继承的。
console.log('foo' in obj) // true
console.log('toString' in obj) // true
  
// hasOwnProperty方法判断是否为对象自身的属性。
console.log(obj.hasOwnProperty('foo')) // true
console.log(obj.hasOwnProperty('toString')) // false
```

## 属性的遍历

```javascript
const obj = { 
  foo: 'Hello', 
  bar : 'World'
}
  
// for...in循环用来遍历一个对象的全部属性。
// * 它遍历的是对象所有可遍历（enumerable）的属性，会跳过不可遍历的属性。
// * 它不仅遍历对象自身的属性，还遍历继承的属性。
for (const key in obj) {
  if (Object.hasOwnProperty.call(obj, key)) {
    const element = obj[key];
    console.log('key为', key, '值为', element)
  }
}
```

## 判断是否为空

```js
object = {}
const isEmpty = Object.keys(object).length === 0
```
