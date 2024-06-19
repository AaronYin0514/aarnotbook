---
tags: [javascript, class]
---
# 类

## 定义类及构造方法

`class 类名`定义类，`constructor`为类的构造方法

```javascript
class Phone {
	constructor(name, price) {
		this.name = name
		this.price = price
	}
}
```

## 创建对象

使用`new`关键字创建对象

```javascript
const p = new Phone("iPhone", 5000)
p.name // "iPhone"
p.price // 5000
```

## 类的实例属性及方法

```javascript
class Phone {
	constructor(name, price) {
		this.name = name
		this.price = price
	}

	call(number) {
		if (!this.addressBook) {
			this.addressBook = []
		}
		const friend = this.addressBook.find(item => item.number === number)
		if (friend) {
			console.log(`正在呼叫${friend.name}...`)
		} else {
			console.log("没有这个联系人...")
		}
	}
}

const p = new Phone("iPhone", 5000)
p.addressBook = [
	{
	name: "Bob",
	number: "13012345678"
	}
]
p.call("13012345678") // 正在呼叫Bob...
```

## set get

`set`与`get`方法可以让调用者像使用属性一样调用方法。在`set`和`get`方法中可以控制变量的赋值。

```javascript
const COST = 1200

class Phone {
	_price = COST

	constructor(name) {
		this.name = name
	}

	set price(newValue) {
		if (newValue < COST) {
			this._price = COST
			return
		}
		this._price = newValue
	}

	get price() {
		if (this._price < COST) {
			return COST
		}
		return this._price
	}
}

const p = new Phone("iPhone")
p.price = 100
console.log(p.price) // 1200
```

## 类的静态属性和方法

```javascript
class Phone{
	//静态属性
	static name = '手机';
	// 静态方法
	static change(){
		console.log("我可以改变世界");
	}
}

let nokia = new Phone();
console.log(nokia.name); // undefined
console.log(Phone.name); // 手机
```







