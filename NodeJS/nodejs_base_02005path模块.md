---
tags: [nodejs]
---

# Path模块

path模块用于对路径的处理，并且提供跨平台兼容性处理。

## 从路径中获取信息

### `dirname`获取文件的父文件夹

```javascript
const path = require("path")
const filePath = "/Users/xxxx/Document/test.js"

path.dirname(filePath) // /Users/xxxx/Document
```

### `basename`获取文件名

```javascript
const path = require("path")
const filePath = "/Users/xxxx/Document/test.js"

path.basename(filePath) // test.js
```

### `extname`获取文件扩展名

```javascript
const path = require("path")
const filePath = "/Users/xxxx/Document/test.js"

path.extname(filePath) // .js
```

## `join`路径的拼接

```javascript
const path = require("path")
const basepath = '/User/xxxx/Document';
const filename = '../test.js';

path.join(basepath, filename); // /User/xxxx/test.js
```

## 将文件和某个文件夹拼接

`path.resolve`将某个文件和文件夹拼接

```javascript
const path = require("path")
const basepath = '/User/xxxx/Document'
const filename = '../node/test.js'

path.resolve(basepath, filename); // /User/xxxx/node/test.js
```

