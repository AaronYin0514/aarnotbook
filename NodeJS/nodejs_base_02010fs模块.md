---
tags: [nodejs]
---

# fs模块

fs模块是Node.JS提供的文件系统模块。提供同步、异步回调函数方式和异步Promise三种方式API，完成文件读写、拷贝和重命名等等操作。

## 获取文件状态

1. 同步读取文件状态

```javascript
const fs = require("fs")

const state = fs.statSync('./foo.js')
console.log(state)
```

结果：

```json
Stats {
  dev: 16777232,
  mode: 33188,
  nlink: 1,
  uid: 501,
  gid: 20,
  rdev: 0,
  blksize: 4096,
  ino: 16544626,
  size: 171,
  blocks: 8,
  atimeMs: 1630910892788.2402,
  mtimeMs: 1630910891550.0383,
  ctimeMs: 1630910891550.0383,
  birthtimeMs: 1630910698123.1174,
  atime: 2021-09-06T06:48:12.788Z,
  mtime: 2021-09-06T06:48:11.550Z,
  ctime: 2021-09-06T06:48:11.550Z,
  birthtime: 2021-09-06T06:44:58.123Z
}
```

2. 异步回调读取文件状态

```javascript
const fs = require("fs")

fs.stat("./foo.js", (err, state) => {
	if (err) {
		console.log(`读取错误：${err}`)
		return
	}
	console.log(state)
})
```

3. 异步Promise读取文件状态

```javascript
const fs = require("fs")

const promise = fs.promises.stat("./foo.js")

promise.then(state => {
	console.log(state)
}).catch(err => {
	console.log(`读取错误：${err}`)
})
```

## 文件的读写

API：

```
fs.readFile(path[, options], callback) // 读取文件
fs.writeFile(file, data[, options], callback) // 在文件中写入内容
```

1. 读取文件内容

content.txt文件

```
Hello World!
```

读取文件：

```javascript
const fs = require("fs")

fs.promises.readFile("./content.txt", {flag: "r", encoding: "utf-8"})
.then(content => {
	console.log(content)
})
.catch(err => {
	console.log("文件读取失败")
})
```

2. 写内容到文件中

```javascript
const fs = require("fs")

fs.promises.writeFile("./content.txt", "Hello,JavaScript!", {flag: "a+"})
.then(() => {
	console.log("文件写入成功")
})
.catch(err => {
	console.log("文件写入失败")
})
```

3. `flag`选项

- **w**：打开文件写入，默认值；
- **w+**：打开文件读写，如果不存在则创建文件；
- **r+**：打开文件读写，如果不存在那么抛出异常；
- **r**: 打开文件读取，读取时的默认值；
- **a**：打开要写入的文件，将流放在文件末尾。如果不存在则创建文件；
- **a+**：打开文件读写，将流放在文件末尾。如果不存在则创建文件。

4. encoding选项

文件读取，如果没有设置，返回`Buffer`

```javascript
const fs = require("fs")
fs.readFileSync("./content.txt", {flag: "r"}) // <Buffer 48 65 6c 6c 6f 20 57 6f 72 6c 64 21>
```

## 判断文件是否存在

```javascript
const fs = require("fs")

const isExist = fs.existsSync("./content.txt")
console.log(isExist)
```

## 复制文件

```javascript
fs.copyFileSync("./content.txt", "./content副本.txt")
```

## 移动/重命名文件

```javascript
fs.renameSync("./content副本.txt", "./Dir/content_copy.txt")
```

## 创建文件夹

```javascript
fs.mkdirSync("new_forder")
```

## 查看文件夹

```javascript
const fs = require("fs")

fs.promises.readdir("./new_forder", {withFileTypes: true})
.then(dirents => {
	dirents.forEach(dirent => {
		console.log("-------")
		console.log(dirent.name)
		console.log(dirent.isDirectory())
		console.log("-------")
	})
})
.catch(err => {
	console.log("读取失败")
})
```





