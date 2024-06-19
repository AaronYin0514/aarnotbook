---
tags: [nodejs]
---

# NodeJS

NodeJS是一个基于V8 JavaScript引擎的JavaScript运行环境。

## 浏览器概念

[[浏览器|浏览器相关概念]]

## 浏览器和Node.JS架构的区别

![浏览器和Node.JS架构的区别](assets/imgs/browser_node_frame.png)

## Node.JS架构

![浏览器和Node.JS架构的区别](assets/imgs/node_frame.png)

1. JavaScript代码会经过V8引擎，再经过Node.JS的Bindings，将任务放到**libuv**的事件循环中；
2. **libuv**（Unicorn Velociraptor）是使用C语言编写的库；
3. **libuv**提供了事件循环、文件系统读写、网络IO、线程池等等内容；

## Node的安装

## Node程序传递参数

创建文件**js_test.js**：

```javascript
console.log(process.argv)
```

执行:

```shell
node js_test.js p1 p2
```

结果：

```
[
  '/usr/local/n/versions/node/14.16.1/bin/node',
  '/Users/xxxx/Documents/Test/Test/js_test.js',
  'p1',
  'p2'
]
```


## Node的输出

```javascript
console.log  //最常用的输入内容的方式:console.log
console.clear  //清空控制台:console.clear
console.trace  //打印函数的调用栈:console.trace
```

## Node的特殊全局对象

- **__dirname**:获取当前文件所在的路径，不包括后面的文件名。
- **__filename**:获取当前文件所在的路径和文件名称，包括后面的文件名称。









