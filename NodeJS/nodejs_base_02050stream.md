---
tags: [nodejs]
---

# Stream

流是连续字节的一种表现形式和抽象概念， 流是可读可写的。流的相关操作有从什么位置开始读取、读到什么位置、一次性读取多少个字节、断点续传等。

## Node.JS四种流类型

- **Writable**: 可以向其写入数据的流(例如 fs.createWriteStream())。  
- **Readable**: 可以从中读取数据的流(例如 fs.createReadStream())。  
- **Duplex**: 同时为Readable和的流Writable(例如 net.Socket)。  
- **Transform**: Duplex可以在写入和读取数据时修改或转换数据的流(例如zlib.createDeflate())。

## Readable

`fs.readFile`一次性读取文件内容到内存，不适合大文件读取。`fs.createReadStream`通过流方式读取文件。

### 1. 创建Readable

```javascript
const fs = require("fs")

const read = fs.createReadStream("./content.txt", {
	start: 2,
	end: 10,
	highWaterMark: 4
})
```

- start：文件读取开始的位置；
- end：文件读取结束的位置；
- highWaterMark：一次性读取字节的长度，默认是64kb；

### 2. 监听文件读取事件 - **open**

**open**文件打开事件

```javascript
read.on("open", (fd) => {
	console.log(fd)
	console.log("文件被打开")
})
```

### 3. 监听文件读取事件 - **data**

**data**文件读取事件

```javascript
read.on("data", (data) => {
	console.log(data.toString())
})
```

### 4. 监听文件读取事件 - **end**

**end**文件读取结束事件

```javascript
read.on("end", () => {
	console.log("文件读取结束")
})
```

### 5. 监听文件读取事件 - **close**

**close**文件关闭事件

```javascript
read.on("close", () => {
	console.log("文件被关闭")
})
```

### 6. 暂停读取

```javascript
read.pause()
```

### 7. 恢复读取

```javascript
read.resume()
```

## Writable

### 1. 创建Writable

```javascript
const fs = require("fs")

const writer = fs.createWriteStream("./content.txt", {
	flags: "a+"
})
```

### 2. 监听文件写入事件-**open**

**open**文件打开事件

```javascript
writer.on("open", (fd) => {
	console.log("文件被打开了")
})
```

### 3. 监听文件写入事件-**finish**

**finish**文件写入完成事件

```javascript
writer.on("finish", () => {
	console.log("文件写入结束")
})
```

### 4. 监听文件写入事件-**close**

**close**文件关闭事件

```javascript
writer.on("close", () => {
	console.log("文件被关闭了")
})
```

### 5. 写入数据

```javascript
writer.write("哈喽", err => {
	console.log(err)
})
```

### 6. 关闭文件

#### `close`方法关闭文件

```javascript
writer.close()
```

#### `end`方法先写入数据，再关闭文件

```javascript
writer.end("Hello,NodeJS")
```

## pipe

将读取的到的**输入流**，添加到**输出流**中写入，可以使用管道`pipe`

```javascript
const fs = require("fs")

const reader = fs.createReadStream("./content.txt")
const writer = fs.createWriteStream("./content_copy.txt")

reader.pipe(writer)
```


