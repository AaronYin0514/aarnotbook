---
tags: [javascript]
---

# 变量

## 变量的定义

[[编程概念#编程概念#变量|变量的定义]]

## 变量命名规则

变量名是大小写英文字母、数字、`$`和`_`的组合名，且不能以数字开头。

## 声明变量

### `var`声明变量及初始化

```javascript
var name;
name = "JavaScript";

var hello = "Hello,JavaScript!";
```

### `let`声明变量及初始化

```javascript
let name;
name = "JavaScript";

let str = "Hello,JavaScript!"
```

### `var` 与 `let` 区别

1. `var`存在[[js_base_01010变量常量#变量提升|变量提升]]的特性；`let`没有，let变量必须先声明，再做其它操作。
2. `var`可以重复定义相同名称的变量；`let`不能。

**使用let，不要使用var。**

### 变量提升

变量提升就像是把所有的变量声明移动到函数或者全局代码的开头位置。

```javascript
hello = "Hello,JavaScript";
var hello;
```

# 常量

[[编程概念#编程概念#常量|常量的概念]]

`const`声明一个常量。常量的值不能改变。

```javascript
const hello = "Hello,JavaScript"
```

## 数据类型

[[js_base_01020基本数据类型|变量的数据类型]]






