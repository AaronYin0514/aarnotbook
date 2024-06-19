---
tags: electron
---

# SQLite3
## 安装

```shell
# npm
npm install sqlite3 -dev

# yarn
yarn add sqlite3 -D
```

### Mac M1芯片安装问题

```shell
npm install sqlite3 --target_arch=arm64
```

### Mac M1芯片yarn安装问题

先使用npm安装依赖，找到`napi-v6-darwin-arm64`文件夹，保存起来，在`package.json`中添加钩子函数

```shell
# napi-v6-darwin-arm64文件夹位置
node_modules/sqlite3/lib/binding/napi-v6-darwin-arm64/node_sqlite3.node
```

```json
...
	"script": {
		...
		"posinstall": "cp -r 备份路径 ./node_modules/sqlite3/lib/binding"
		...
	}
...
```

## 基本使用

[node-sqlite3官方文档](https://github.com/TryGhost/node-sqlite3/wiki/API)

```javascript
const sqlite3 = require('sqlite3').verbose();
const db = new sqlite3.Database(':memory:');

db.serialize(() => {
    db.run("CREATE TABLE lorem (info TEXT)");

    const stmt = db.prepare("INSERT INTO lorem VALUES (?)");
    for (let i = 0; i < 10; i++) {
        stmt.run("Ipsum " + i);
    }
    stmt.finalize();

    db.each("SELECT rowid AS id, info FROM lorem", (err, row) => {
        console.log(row.id + ": " + row.info);
    });
});

db.close();
```

## 创建数据库

```javascript

```

## 创建更新表格

## 插入数据

## 查询数据

## 删除数据


