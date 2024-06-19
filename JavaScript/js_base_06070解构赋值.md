---
tags：javascript
---

# 解构赋值

ES6 允许按照一定模式从数组和对象中提取值，对变量进行赋值，这被称为解构赋值。这大大简化了我们的代码。

## 数组的解构赋值

### 解构赋值

```javascript
const [x, y] = ["JavaScript", "Python", "Ruby"]
console.log(x) // "JavaScript"
console.log(y) // "Python"
```

### 嵌套结构

```javascript
const [x, [y]] = ["JavaScript", ["Python", "Ruby"]]
console.log(x) // "JavaScript"
console.log(y) // "Python"
```

### 对应的元素不存在赋值`undefined`

```javascript
const [x, y, z] = ["JavaScript", "Python"]
console.log(x) // "JavaScript"
console.log(y) // "Python"
console.log(z) // undefined
```

### 默认值

```javascript
const [x, y, z = "Ruby"] = ["JavaScript", "Python"]
console.log(x) // "JavaScript"
console.log(y) // "Python"
console.log(z) // "Ruby"
```

### 忽略前面的元素

```javascript
const [, , z] = ["JavaScript", "Python", "Ruby"]
console.log(z) // "Ruby"
```

## 对象的解构赋值

### 解构赋值

```javascript
const { name, age } = {name: "张三", age: 18}
console.log(name) // "张三"
console.log(age) // 18
```

### 嵌套结构

```javascript
const { name, age, address: {city} } = {name: "张三", age: 18, address: {city: "上海", street: "闵行区"}}
console.log(name) // "张三"
console.log(age) // 18
console.log(city) // "上海"
```

### 对应的元素不存在赋值`undefined`

```javascript
const { name, age, address } = {name: "张三", age: 18}
console.log(name) // "张三"
console.log(age) // 18
console.log(address) // undefined
```

### 默认值

```javascript
const { name, age = 18 } = {name: "张三"}
console.log(name) // "张三"
console.log(age) // 18
```

