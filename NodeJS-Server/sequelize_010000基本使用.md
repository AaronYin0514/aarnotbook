---
tags: nodejs, sequelize
---

# Sequelize基本使用

- [官网](https://sequelize.org)
- [中文站](https://www.sequelize.com.cn)

## 创建项目 - 脚手架

### 安装CLI

```shell
# using npm  
npm init
npm install sequelize
npm install --save-dev sequelize-cli  
# using yarn
yarn init
yarn add sequelize
yarn add sequelize-cli --dev
```

### 创建项目

```shell
# using npm
npx sequelize-cli init
# using yarn
yarn sequelize-cli init
```

### 目录结构介绍

-   `config`, 包含配置文件,它告诉CLI如何连接数据库
-   `models`,包含你的项目的所有模型
-   `migrations`, 包含所有迁移文件
-   `seeders`, 包含所有种子文件

### 连接数据库

#### 开启mysql服务

```shell
# mac
mysql.server start
```

#### 配置数据库

```json
{
	"development": {
		"username": "root",
		"password": "******",
		"database": "words_dev",
		"host": "127.0.0.1",
		"dialect": "mysql"
	},
	"test": {
		"username": "root",
		"password": null,
		"database": "database_test",
		"host": "127.0.0.1",
		"dialect": "mysql"
	},
	"production": {
		"username": "root",
		"password": null,
		"database": "database_production",
		"host": "127.0.0.1",
		"dialect": "mysql"
	}
}
```

#### 创建数据库

初始化项目后，还没有创建数据库，运行下面命令

```shell
npx sequelize-cli db:create
```


## 迁移命令

### 运行迁移

```shell
# using npm
npx sequelize-cli db:migrate
# using yarn
yarn sequelize-cli db:migrate
```

### 撤销迁移

恢复最近的迁移

```shell
# using npm
npx sequelize-cli db:migrate:undo
# using yarn
yarn sequelize-cli db:migrate:undo
```

撤消所有迁移,可以恢复到初始状态

```shell
npx sequelize-cli db:migrate:undo:all
```

撤销到指定迁移，指定`--to`参数

```shell
# using npm
npx sequelize-cli db:migrate:undo:all --to XXXXXXXXXXXXXX-create-posts.js
# using yarn
yarn sequelize-cli db:migrate:undo:all --to XXXXXXXXXXXXXX-create-posts.js
```

## 模型

### 创建模型

- `name`：模型的名称
- `attributes`：模型的属性列表

```shell
# using npm
npx sequelize-cli model:generate --name User --attributes firstName:string,lastName:string,email:string
# using yarn
yarn sequelize-cli model:generate --name User --attributes firstName:string,lastName:string,email:string
```

### 模型扩展/修改字段

```shell
npx sequelize-cli migration:generate --name change-user
```

## 种子


