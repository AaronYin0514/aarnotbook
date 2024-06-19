---
tags: javascript, object
---

# Object

## 冻结对象 Object.freeze()

**`Object.freeze()`** 方法可以$\color{red} {冻结}$一个对象。一个被冻结的对象再也不能被修改；冻结了一个对象则不能向这个对象添加新的属性，不能删除已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性，以及不能修改已有属性的值。此外，冻结一个对象后该对象的原型也不能被修改。`freeze()` 返回和传入的参数相同的对象。

```javascript
const obj = {
  prop: 42
};

Object.freeze(obj);

obj.prop = 33;
// Throws an error in strict mode

console.log(obj.prop);
// expected output: 42
```

## Object.assign()

**`Object.assign()`** 方法将所有可枚举（`Object.propertyIsEnumerable()` 返回 true）的自有（`Object.hasOwnProperty()` 返回 true）属性从一个或多个源对象复制到目标对象，返回修改后的对象。

```javascript
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// Expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget === target);
// Expected output: true
```

## Object.keys()

参数：要返回其枚举自身属性的对象
返回值：一个表示给定对象的所有可枚举属性的字符串数组

```js
let person = {name:"张三",age:25,address:"深圳",getName:function(){}}
Object.keys(person) // ["name", "age", "address","getName"]

let arr = [1,2,3,4,5,6];
Object.keys(arr); // ["0", "1", "2", "3", "4", "5"]
```


