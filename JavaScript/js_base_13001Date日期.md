# Date
## 创建Date

#### 1 创建当前日期

```javascript
// 显示当前的日期/时间
const now = new Date(); 
```

#### 2 时间戳创建日期

> **时间戳**：自 1970-01-01 00:00:00 以来经过的毫秒数，该整数被称为时间戳。

`new Date(milliseconds)`创建一个 `Date` 对象，其时间等于 1970 年 1 月 1 日 UTC+0 之后经过的毫秒数（1/1000 秒）。

```javascript
// 0 表示 01.01.1970 UTC+0 
let Jan01_1970 = new Date(0);
```

#### 3 字符串创建日期

`new Date(datestring)`如果只有一个参数，并且是字符串，那么它会被自动解析。

```javascript
let date = new Date("2017-01-26");
```

#### 4 年月[日时分秒]创建日志

`new Date(year, month, date, hours, minutes, seconds, ms)`使用当前时区中的给定组件创建日期。

-   `year` **必传**。必须是四位数：`2013` 是合法的，`98` 是不合法的。
-   `month` **必传**。计数从 `0`（一月）开始，到 `11`（十二月）结束。
-   `date` 是当月的具体某一天，如果缺失，则为默认值 `1`。
-   如果 `hours/minutes/seconds/ms` 缺失，则均为默认值 `0`。

```javascript
new Date(2011, 0, 1, 0, 0, 0, 0); // 1 Jan 2011, 00:00:00 
new Date(2011, 0, 1); // 同样，时分秒等均为默认值 0
```

## 时间戳

#### 1 时间戳创建日期

```javascript
// 0 表示 01.01.1970 UTC+0 
let Jan02_1970 = new Date(24 * 3600 * 1000);
```

#### 2 获取时间戳

`getTime()`

```javascript
let date = new Date("2017-01-26");
// 获取时间戳
let t = date.getTime();
```

#### 3 获取当前时间戳

`Date.now()`

```javascript
// 返回当前时间戳
let t = Date.now()
```

## 格式化解析

`Date.parse(str)`方法可以从一个字符串中读取日期。

字符串的格式应该为：`YYYY-MM-DDTHH:mm:ss.sssZ`，其中：

-   `YYYY-MM-DD` —— 日期：年-月-日。
-   字符 `"T"` 是一个分隔符。
-   `HH:mm:ss.sss` —— 时间：小时，分钟，秒，毫秒。
-   可选字符 `'Z'` 为 `+-hh:mm` 格式的时区。单个字符 `Z` 代表 UTC+0 时区。

```javascript
let ms = Date.parse('2012-01-26T13:51:50.417-07:00'); 
alert(ms); // 1327611110417 (时间戳)
```

## 日期转数字-差值

当 `Date` 对象被转化为数字时，得到的是对应的时间戳，与使用 `date.getTime()` 的结果相同：

```javascript
let date = new Date(); 
alert(+date); // 以毫秒为单位的数值，与使用 date.getTime() 的结果相同
```

日期可以相减，相减的结果是以毫秒为单位时间差。

```javascript
let start = new Date(); // 开始测量时间  

// do the job 
for (let i = 0; i < 100000; i++) {   
	let doSomething = i * i * i; 
}  

let end = new Date(); // 结束测量时间  
alert( `The loop took ${end - start} ms` );
```

## 访问日期

```javascript
getFullYear() // 获取年份（4 位数）
getMonth() // 获取月份，从 0 到 11。
getDate() // 获取当月的具体日期，从 1 到 31，这个方法名称可能看起来有些令人疑惑。
getHours() // 获取小时
getMinutes() // 获取分钟
getSeconds() // 获取秒
getMilliseconds() // 获取毫秒
getDay() // 获取一周中的第几天，从 `0`（星期日）到 `6`（星期六）。
```

## 设置日期

```javascript
setFullYear(year, [month], [date])
setMonth(month, [date])
setDate(date)
setHours(hour, [min], [sec], [ms])
setMinutes(min, [sec], [ms])
setSeconds(sec, [ms])
setMilliseconds(ms)
setTime(milliseconds)
```