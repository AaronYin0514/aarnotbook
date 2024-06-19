---
tags: [javascript]
---

# 函数

## 定义

```javascript
function hello() {
	console.log("Hello,JavaScript!")
}

function add(x, y) {
	return x + y
}
```

`function`关键字定义函数。*hello*和*add*是函数的**函数名**。`x, y`是函数的**参数**。`return`的结果是函数的**返回值**，没有`return`的函数默认返回`undefined`。

## 调用

```javascript
hello()
let res = add(1, 2) // 3
```

## 参数

### 默认参数

ES6引入默认参数。定义函数参数时，使用`=`设置参数的默认值，如果调用函数时没有传入该参数，会取默认值。

```javascript
function add(x = 0, y = 0) {
	return x + y
}

add() // 0
```

### `arguments`参数列表

JavaScript函数的调用允许传入任意个参数。函数内可通过`arguments`获取参数列表。

```javascript
function add() {
	if (arguments.length < 2) {
		return 0
	}
	return arguments[0] + arguments[1]
}

add(1) // 0
add(1, 2, 3, 4) // 3
```

### rest参数

ES6 引入 rest 参数，用于获取函数未定义的的实参，用来代替 arguments。rest 参数必须要放到参数最后。

```javascript
function add(x, y, ...rest) {
	let result = x + y
	for (const p of rest) {
		result += p
	}
	return result
}

add(1, 2, 3, 4, 5) // 15
```



	
