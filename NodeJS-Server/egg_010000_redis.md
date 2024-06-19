---
tags: nodejs egg redis egg-redis
---

# egg-redis

## Redis简介

REmote DIctionary Server(Redis) 是一个由 Salvatore Sanfilippo 写的 key-value 存储系统，是跨平台的非关系型数据库。

Redis 是一个开源的使用 ANSI C 语言编写、遵守 BSD 协议、支持网络、可基于内存、分布式、可选持久性的键值对(Key-Value)存储数据库，并提供多种语言的 API。

Redis 通常被称为数据结构服务器，因为值（value）可以是 _**字符串(String)、哈希(Hash)、列表(list)、集合(sets)和有序集合(sorted sets)**_ 等类型。

## egg-redis

**github：[egg-redis](https://github.com/eggjs/egg-redis)**

Egg框架下基于 `ioredis` 的Redis客户端（支持Redis Portocal），如果你想了解更多用法，可以参考 [ioredis](https://github.com/luin/ioredis)的文档。

## 安装

```shell
npm i egg-redis --save

# 或者使用 yarn 进行安装
yarn add egg-redis --save
```

## 启用redis插件：

```javascript
// config/plugin.js
exports.redis = {
  enable: true,
  package: 'egg-redis',
};
```

## 配置

在 `config/config.${env}.js` 配置各个环境的redis连接信息

### 单数据源

```javascript
config.redis = {
  client: {
    port: 6379,          // Redis port
    host: '127.0.0.1',   // Redis host
    password: 'auth',
    db: 0,
  },
}
```

### 多数据源

```javascript
config.redis = {
  clients: {
    foo: {                 // instanceName. See below
      port: 6379,          // Redis port
      host: '127.0.0.1',   // Redis host
      password: 'auth',
      db: 0,
    },
    bar: {
      port: 6379,
      host: '127.0.0.1',
      password: 'auth',
      db: 1,
    },
  }
}
```

## 使用

### 单数据源

可以在 `controller` 中使用 `app.redis` 获取redis实例，查看[ioredis](https://github.com/luin/ioredis#basic-usage)更多用法。

```javascript
// app/controller/home.js

module.exports = app => {
  return class HomeController extends app.Controller {
    async index() {
      const { ctx, app } = this;
      // set
      await app.redis.set('foo', 'bar');
      // get
      ctx.body = await app.redis.get('foo');
    }
  };
};
```

### 多数据源

如果配置了多个redis，可以通过 `app.redis.get(instanceName)` 来获取redis实例，具体用法：

```javascript
// app/controller/home.js

module.exports = app => {
  return class HomeController extends app.Controller {
    async index() {
      const { ctx, app } = this;
      // set
      await app.redis.get('instance1').set('foo', 'bar');
      // get
      ctx.body = await app.redis.get('instance1').get('foo');
    }
  };
};
```

## 常用操作

```javascript
// get
const foo = await app.redis.get(key)

// set
await app.redis.set(key, value)
// set同时设置有效期，单位：秒
await app.redis.set(key, value, 'EX', seconds)
// 设置有效期
await app.redis.expire(key, seconds)

// 删除
await app.redis.del(key)

// 删除所有 ！！！慎用
await app.redis.flushall()
```