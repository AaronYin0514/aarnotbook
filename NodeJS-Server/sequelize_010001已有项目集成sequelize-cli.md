---
tags: nodejs, sequelize
---

# 已有项目集成sequelize-cli

## 安装 sequelize-cli

```shell
npm install --save-dev sequelize-cli
```

## 集成

在项目中，我们想将所有数据库 Migrations 相关的内容都放在 `database` 目录下，在项目根目录下新建一个 `.sequelizerc` 配置文件：

```javascript
'use strict';

const path = require('path');

module.exports = {
  config: path.join(__dirname, 'database/config.json'),
  'migrations-path': path.join(__dirname, 'database/migrations'),
  'seeders-path': path.join(__dirname, 'database/seeders'),
  'models-path': path.join(__dirname, 'app/model'),
};
```

初始化 Migrations 配置文件和目录

```shell
npx sequelize init:config
npx sequelize init:migrations
```

执行完后会生成 `database/config.json` 文件和 `database/migrations` 目录，修改 `database/config.json` 中的数据库配置：

```json
{
  "development": {
    "username": "root",
    "password": null,
    "database": "egg-sequelize-doc-default",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "test": {
    "username": "root",
    "password": null,
    "database": "egg-sequelize-doc-unittest",
    "host": "127.0.0.1",
    "dialect": "mysql"
  }
}
```


