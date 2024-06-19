---
tags: [javascript]
---

# Number

## 字面量定义Number

```javascript
let a = 1 // 整数1
let b = 3.14 // 浮点数3.14
let c = 1.23e3 // 科学计数法， 1.23 * 1000
let d = -99 // 负数
let e = 0b10000000000 // 二进制 1024
let f = 0o2000 // 八进制 1024
let g = 0x400 // 十六进制 1024
```

## Number的范围

JavaScript 能够准确表示的整数范围在`-2^53`到`2^53`之间（不含两个端点），超过这个范围，无法精确表示这个整数。

```
var biggestInt = Number.MAX_SAFE_INTEGER; //9007199254740991
var smallestInt = Number.MIN_SAFE_INTEGER; //-9007199254740991
```

## NaN

全局属性 `NaN` 的值表示不是一个数字（Not-A-Number）。和 `Number.NaN` 的值一样。通常是在计算失败或者将一个字符串解析成数字失败时返回`NaN`。

```javascript
0 / 0 // NaN

parseInt("a") // NaN
```

### 判断一个值是否是NaN

```javascript
isNaN(NaN) // true
Number.isNaN(NaN) // true
```

`isNaN` 与 `Number.isNaN`的区别：

- `isNaN`: 如果当前值是`NaN`，或经过强制转换类型后是`NaN`，都返回`true`
- `Number.isNaN`: 仅当前值是`NaN`时返回`true`

```javascript
isNaN('hello world');        // true
Number.isNaN('hello world'); // false
```

## Infinity

全局属性 `Infinity` 是一个数值，表示无穷大。初始值是`Number.POSITIVE_INFINITY`。

```javascript
console.log(Infinity); /* Infinity */
console.log(Infinity + 1); /* Infinity */
console.log(Math.pow(10, 1000)); /* Infinity */
console.log(Math.log(0)); /* -Infinity */
console.log(1 / Infinity); /* 0 */
```

### `isFinite`检测是否为有限数

```javascript
Number.isFinite(100) // true
Number.isFinite(100 / 0) // false
Number.isFinite(Infinity) // false
```

## 字符串转数字

```javascript
Number('12.3')    // 12.3
Number('12.00')   // 12
Number('123e-1')  // 12.3
Number('')        // 0
Number(null)      // 0
Number('0x11')    // 17
Number('0b11')    // 3
Number('0o11')    // 9
Number('foo')     // NaN
Number('100a')    // NaN
Number('-Infinity') //-Infinity
```

### `parseInt` &  `parseFloat`

```javascript
Number.parseInt("3.14abc") // 3
Number.parseFloat("3.14abc") // 3.14
```

## 数字转字符串

```javascript
let a = 1024
a.toString() // '1024'
a.toString(2) // '10000000000' 转二进制字符串
a.toString(8) // '2000' 转8进制字符串
a.toString(16) // '400' 转16进制字符串
```

## EPSILON

`Number.EPSILON`是 JavaScript 表示的最小精度，它的值接近于 *2.2204460492503130808472633361816E-16*。

```javascript
function equal(a, b) {
	if (Math.abs(a - b) < Number.EPSILON) {
		return true
	} else {
		return fase
	}
}
console.log(0.1 + 0.2 === 0.3) // false
console.log(equal(0.1 + 0.2, 0.3)) // true
```

## `isInteger`判读一个数是否是整数

```javascript
Number.isInteger(3) // true
Number.isInteger(3.14) // false
```

## `Math.sign`判断一个数是整数、负数或0

 ```
Math.sign(3) // 1
Math.sign(0) // 0
Math.sign(-3) // -1
 ```

## `Math.trunc`去掉小数部分

```javascript
Math.trunc(3.14) // 3
```


# BigInt

## 定义BigInt

`BigInt`是一种内置对象，它提供了一种方法来表示大于 `253 - 1` 的整数。

```
let a = BigInt(Number.MAX_SAFE_INTEGER)
let b = 10n
```



