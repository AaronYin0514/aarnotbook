---
tags: [javascript]
---

# 类的继承

通过`extends`继承父类。通过`super`调用父类方法。

```javascript
class Phone{
//构造方法
	constructor(brand, price){
		this.brand = brand;
		this.price = price;
	}

	//父类的成员属性
	call(){
		console.log("我可以打电话!!");
	}
}

class SmartPhone extends Phone {
	//构造方法
	constructor(brand, price, color, size){
		super(brand, price);// Phone.call(this, brand, price)
		this.color = color;
		this.size = size;
	}

	photo(){
		console.log("拍照");
	}

	playGame(){
		console.log("玩游戏");
	}

	call(){
		super.call()
		console.log('我可以进行视频通话');
	}
}

const xiaomi = new SmartPhone('小米',799,'黑色','4.7inch');
xiaomi.call();
xiaomi.photo();
xiaomi.playGame();
```