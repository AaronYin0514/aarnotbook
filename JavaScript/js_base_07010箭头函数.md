---
tags: [javascript]
---

# 箭头函数

## 定义及调用

```javascript
const add = (x, y) => {
	return x + y
}

add(1, 2) // 3
```

## 箭头函数的简写方式

当函数体只有一个表达式时，可省略`return` 和 `{}`

```javascript
const add = (x, y) => x + y
add(1, 2) // 3
```

如果直接返回一个对象，需要给对象加一个括号：

```javascript
const person = (name, age) => ({name, age})
person("张三", 18) // { name: '张三', age: 18 }
```

## 箭头函数的`this`

箭头函数的 `this` 是静态的.  `this` 始终指向函数声明时所在作用域下的 `this` 的值。

```javascript
let person = {
	birth: 1990,
	getAge: function () {
		let b = this.birth // 1990
		let fn = function () {
			return new Date().getFullYear() - this.birth; // this指向window或undefined
		};
		return fn()
	}
}

person.getAge() // NaN
```

箭头函数修复了该问题：

```javascript
let person = {
	birth: 1990,
	getAge: function () {
		let b = this.birth; // 1990
		let fn = () => new Date().getFullYear() - this.birth; // this指向obj对象
		return fn();
	}
};
person.getAge() // 31
```

## 与`function`函数的区别

### 不能作为构造实例化对象

```javascript
let Person = (name, age) => {
this.name = name;
this.age = age;
}
let me = new Person('xiao',30); // 报❌
console.log(me);
```

### 不能使用 arguments 变量

```javascript
let fn = () => {
	console.log(arguments);
}

fn(1,2,3); // ❌ 不是想要的结果
```



