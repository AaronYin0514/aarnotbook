---
tags: [nodejs]
---

# Buffer

Buffer相当于是一个字节的数组，数组中的每一项对于一个字节的大小.

## 创建Buffer

### from

```javascript
// 创建Buffer
const b1 = new Buffer.from("Hello,NodeJS")
// 指定编码，默认为 utf8
const b2 = new Buffer.from("哈喽，NodeJS！", "utf16le")

console.log(b1) // <Buffer 48 65 6c 6c 6f 2c 4e 6f 64 65 4a 53>
console.log(b2) // <Buffer c8 54 bd 55 0c ff 4e 00 6f 00 64 00 65 00 4a 00 53 00 01 ff>
```

### alloc

创建了一个8位长度的Buffer，里面所有的数据默认为00

```javascript
const b3 = Buffer.alloc(8)
console.log(b3) // <Buffer 00 00 00 00 00 00 00 00>
```

填充字符

```javascript
const b3 = Buffer.alloc(8)
b3[0] = 'w'.charCodeAt()
b3[1] = 100
b3[2] = 0x66

console.log(b3) // <Buffer 00 00 00 00 00 00 00 00>
```

## Buffer转字符串

```javascript
const b1 = new Buffer.from("Hello,NodeJS")
const b2 = new Buffer.from("哈喽，NodeJS！", "utf16le")
console.log(b1.toString()) // Hello,NodeJS
console.log(b2.toString("utf16le")) // 哈喽，NodeJS！
```

