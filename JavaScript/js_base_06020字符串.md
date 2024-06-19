---
tags: [javascript, string]
---
# 字符串

## 创建字符串

### 定义

```javascript
const str = "Hello,JavaScriptWorld!";
console.log(str);
```

### 定义多行字符串

```javascript
const peachBlossom = '桃花坞里桃花庵，\
桃花庵下桃花仙；\
桃花仙人种桃树，\
又摘桃花卖酒钱。';  
console.log(peachBlossom);
```

### 字符串模版

```javascript
let name = "Bob", time = "today";
let sayHello = `Hello ${name}, how are you ${time}?`
console.log(sayHello)
```

### 重复

```javascript
const str = 'hello_'.repeat(3)
console.log(str) //hello_hello_hello_
```

### 补全

```javascript
const str1 = '1'.padStart(8, '0')
console.log(str1) // 00000001

const str2 = '12'.padStart(10, 'YYYY-MM-DD')
console.log(str2) // YYYY-MM-12


const str3 = '1'.padEnd(8, '0')
console.log(str3) // 10000000
```

## 字符串长度

```javascript
const str = 'Hello,JavaScriptWorld!';
const length = str.length;
console.log(str, '长度是', length) // Hello,JavaScriptWorld! – "长度是" – 22
```

## 字符串比较

```javascript

```

## 拼接

### +运算符

```javascript
const str1 = 'Hello,';
const str2 = 'JavaScriptWorld!';
const str3 = str1 + str2;
console.log(str3) // Hello,JavaScriptWorld!
```

### concat

```javascript
const str1 = "Hello";
const str2 = str1.concat(',', 'JavaScriptWorld', '!');
console.log(str2); //Hello,JavaScriptWorld!
```

### join

```javascript
const arr = ['hello', ',', 'world', '!']
const str = arr.join('')
console.log(str) // hello,world!
```

## 判断字符串是否包含指定字符串

### `indexOf`

```javascript
// indexOf 查找指定字符串。如果有，返回指定字符串下标索引；否则返回-1。
const str = "Hello,World!";
const index = str.indexOf(",")

if (index === -1) {
  console.log("不包含该字符串")
} else {
  console.log("包含该字符串，下标索引是", index)
}
```

### `includes`

```javascript
// includes 返回布尔值，表示是否找到了参数字符串。
const str = "Hello,World!";

if (str.includes("#")) {
  console.log("不包含该字符串")
} else {
  console.log("包含该字符串")
}
```

### 前缀`startsWith`

```javascript
const url = "https://www.google.com"
if (url.startsWith("https://")) {
  console.log("这是一个URL")
}
```

### 后缀`endsWith`

```javascript
const url = "./imgs/20010101145920.jpg"  
if (url.endsWith("jpg")) {  
 	console.log("这是一张图片")  
}
```

## 查找

### `indexOf`

```javascript
// indexOf 查找指定字符串。如果有，返回指定字符串下标索引；否则返回-1。
// 两个参数
// 第一个是要搜索的字符串
// 第二个是要从哪里开始搜索
const str = "Hello,World!";
  
const index1 = str.indexOf(",")

if (index1 === -1) {
  console.log("不包含该字符串")
} else {
  console.log("包含该字符串，下标索引是", index1)
}

const index2 = str.indexOf(",", 6)

if (index2 === -1) {
  console.log("不包含该字符串")
} else {
  console.log("包含该字符串，下标索引是", index2)
}
```

### `lastIndexOf`

```javascript
// lastIndexOf从后向前查找，返回指定字符串下标索引  
const str = "Good good study, day day up!";  
  
const index = str.lastIndexOf("day")  
if (index === -1) {  
 	console.log("不包含该字符串")  
} else {  
 	console.log("包含该字符串，下标索引是", index)  
}
```

### `search`


```javascript
// search方法查找匹配字符串第一次出现的位置，如果没有找到，则返回 -1。   
// search方法仅有一个参数，定义匹配模式。为正则表达式（RegExp 对象）。  
// 如果参数不是 RegExp 对象，则JavaScript会使用RegExp函数把它转换为RegExp对象。  
const str = "当前时间：2012-01-31 09:00:22";  
const dateRegular = /(?:19|20)[0-9][0-9]-(?:(?:0[1-9])|(?:1[0-2]))-(?:(?:[0-2][1-9])|(?:[1-3][0-1])) (?:(?:[0-2][0-3])|(?:[0-1][0-9])):[0-5][0-9]:[0-5][0-9]/;  
const index = str.search(dateRegular)  
if (index === -1) {  
 	console.log("不包含该字符串")  
} else {  
 	console.log("包含该字符串，下标索引是", index)  
}  
  
const str2 = "Good good study, day day up!"  
const index2 = str2.search('day')  
if (index2 === -1) {  
 	console.log("不包含该字符串")  
} else {  
 	console.log("包含该字符串，下标索引是", index2)  
}
```

### `match`

```javascript
//match方法查找匹配的字符串，以数组的形式返回。  
const str = "爸爸的生日是1月31日。妈妈的生日是10月20日。";  
const dateRegular = /\d+月\d+日/;  
const result = str.match(dateRegular)  
console.log(result)
```

## 字符串截取

### `substring`

```javascript
// substring方法都是根据指定的起止下标位置来截取字符串，  
// 它包含两个参数，第一个参数表示起始下标，第二个参数表示结束下标。  
// 如果第一个参数值比第二个参数值大，substring先交换两个参数  
const str = "Good good study, day day up!";  
const subStr = str.substring(10, 15)  
console.log(subStr) // study
```

### `slice`

```javascript
// slice方法都是根据指定的起止下标位置来截取字符串，  
// 它包含两个参数，第一个参数表示起始下标，第二个参数表示结束下标。  
// 如果第一个参数值比第二个参数值大，返回空字符串。  
const str = "Good good study, day day up!";  
const subStr = str.slice(10, 15)  
console.log(subStr) // study
```

## 字符串替换

### `replace`

```javascript
const p = 'The quick brown fox jumps over the lazy dog. If the dog reacted, was it really lazy?';

console.log(p.replace('dog', 'monkey'));
// expected output: "The quick brown fox jumps over the lazy monkey. If the dog reacted, was it really lazy?"


const regex = /Dog/i;
console.log(p.replace(regex, 'ferret'));
// expected output: "The quick brown fox jumps over the lazy ferret. If the dog reacted, was it really lazy?"
```

```javascript
var s = 'javascript is script , is not java.';  
// 在函数f中  
// 第一个参数表示每次匹配的文本  
// 第二个参数表示第一个小括号的子表达式所匹配的文本，即单词的首字母，  
// 第二个参数表示第二个小括号的子表达式所匹配的文本。  
var f = function ($1,$2,$3) {  
 	return $2.toUpperCase() + $3;  
}
var a = s.replace(/\b(\w)(\w*)\b/g, f);  //匹配文本并进行替换  
console.log(a); // JavaScript Is Script ， Is Not Java.
```

## 转大小写

```javascript
const str = "Good good study, day day up!";  
  
// 转大写  
const upperCase = str.toUpperCase();  
console.log(upperCase)  
  
// 转小写  
const lowerCase = str.toLowerCase()  
console.log(lowerCase)
```

## 分割

```javascript
const str = "jpg|bmp|gif|ico|png";  
const arr = str.split("|");  
console.log(arr) // ["jpg", "bmp", "gif", "ico", "png"]
```

## trim

```javascript
var str = "   Runoob.   ";
str = str.trim();
```

## 路径

### 获取父文件夹

```javascript
// 获取路径文件夹

function basepath(path) {

    return path.substring(0, path.lastIndexOf("/"))

}  
  
const path = '/Users/admin/document/vidoes/20100105.mp4'

console.log(basepath(path))
```

### 获取文件名

```javascript
// 获取文件名

function basename(path) {
    return path.substring(path.lastIndexOf("/") + 1)
}  
  
const path = '/Users/admin/document/vidoes/20100105.mp4'
console.log(basename(path))
```

### 获取文件后缀

```javascript
// 获取 .后缀名
function extname(path) {
    const index = path.lastIndexOf(".")
    if (index == -1) {
        return ""
    }
    return path.substring(path.lastIndexOf("."))
}
  
// 只获取后缀名
function extname1(path) {
    const index = path.lastIndexOf(".")
    if (index == -1) {
        return ""
    }
    return path.substring(index + 1)
}

  
const path = '/Users/admin/document/vidoes/20100105.mp4'
console.log(extname(path))
console.log(extname1(path))
```

## 字符串（String）转数字（Number）

![[js_base_06010number_bigint#Number#字符串转数字|字符串（String）转数字（Number）]]

## 字符串编码

### 字符串URL编码/解码

#### `encodeURI`编码与`decodeURI`解码

`encodeURI`和`decodeURI`用于对整个 URL 直接编码/解码，但不会对**ASCII字母** 、**数字** 、 **~ ! @ # $ & * ( ) = : / , ; ? + ‘** 进行编码/解码

```javascript
// encodeURI 编码
let str = 'http://www.cnblogs.com/season-huang/some other thing'
let res = encodeURI(str)
console.log(res) // http://www.cnblogs.com/season-huang/some%20other%20thing

// decodeURI 解码
str = 'http://www.cnblogs.com/season-huang/some%20other%20thing'
res = decodeURI(str)
console.log(res) // http://www.cnblogs.com/season-huang/some other thing
```

#### `encodeURIComponent`编码和`decodeURIComponent`解码

`encodeURIComponent`和`decodeURIComponent`对URL的组成部分进行编码/解码，因此会对全部字符进行编码/解码

```javascript
// encodeURIComponent 编码
let str = '测试:/'
let res = encodeURIComponent(str)
console.log(res) // %E6%B5%8B%E8%AF%95%3A%2F

// decodeURIComponent 解码
str = '%E6%B5%8B%E8%AF%95%3A%2F'
res = decodeURIComponent(str)
console.log(res) // 测试:/
```



## 正则表达式

