---
tags: [javascript]
---

# 布尔值类型

## 定义布尔值

```javascript
let a = true;
let b = false;
```

## `!!`类型转换

```javascript
let a = "Hello,JavaScript!"
let b = !!a; // true
```

值`0`，`-0`，`null`，`NaN`，`undefined`，或空字符串（`""`）转换为布尔值为false；其它为true，包括空组数`[]`和空对象`{}`

```
console.log(!!(0)) // false
console.log(!!(-0)) // false
console.log(!!(null)) // false
console.log(!!(false)) // false
console.log(!!(NaN)) // false
console.log(!!(undefined)) // false
console.log(!!("")) // falsev
console.log(!!([])) // true
console.log(!!({})) // true
```



