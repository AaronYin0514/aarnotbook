---
tags: js javascript timer
---

# 定时器

## `setTimeout`

`setTimeout`设置一个定时器，在定时器到期后执行一次函数或代码段

```javascript
// 创建
var timeoutId = window.setTimeout(code[, delay]);

// 清理
clearTimeout(timeoutId);
```

-   `timeoutId`: 定时器ID
-   `func`: 延迟后执行的函数
-   `code`: 延迟后执行的代码字符串，不推荐使用原理类似eval()
-   `delay`: 延迟的时间（单位：毫秒），默认值为0

## `setInterval`

`setInterval`以固定的时间间隔重复调用一个函数或者代码段

```javascript
// 创建
var intervalId = window.setInterval(code, delay);

// 清理
clearInterval(intervalId);
```

-   `intervalId`: 重复操作的ID
-   `func`: 延迟调用的函数
-   `code`: 代码段
-   `delay`: 延迟时间，没有默认值

