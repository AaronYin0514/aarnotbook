---
tags: [nodejs]
---

# HTTP模块

## 创建HTTP服务器

1. `createServer`方法创建

```
const http = require("http")

const server = http.createServer((req, res) => {
	res.end("Hello World!")
})
```

2. 回调参数

 创建Server时需要一个回调函数，回调函数有两个参数:

- **req**: request请求对象，包含请求相关的信息;
- **res**: response响应对象，包含我们要发送给客户端的信息;

##  监听主机和端口号

1. `lesson`

Server通过listen方法来开启服务器，并且在某一个主机和端口上监听网络请求

```javascript
server.listen(8888, () => {
	console.log("服务器已经启动")
})
```

2. 参数

listen函数有三个参数:

- **端口port**: 可以不传, 系统会默认分配端;
- **主机host**: 通常传入localhost、ip地址127.0.0.1、或者ip地址0.0.0.0，默认是0.0.0.0;  
	- **localhost**:本质上是一个域名，通常情况下会被解析成127.0.0.1;  
	- **127.0.0.1**:回环地址(Loop Back Address)，表达的意思其实是我们主机自己发出去的包，直接被自己接收;
		- 正常的数据库包经常 应用层 - 传输层 - 网络层 - 数据链路层 - 物理层 ;  
		- 而回环地址，是在网络层直接就被获取到了，是不会经常数据链路层和物理层的; u
		- 比如我们监听127.0.0.1时，在同一个网段下的主机中，通过ip地址是不能访问的;
	- **0.0.0.0**:  
		- 监听IPV4上所有的地址，再根据端口找到不同的应用程序;  
		- 比如我们监听 0.0.0.0时，在同一个网段下的主机中，通过ip地址是可以访问的;
- **回调函数**:服务器启动成功时的回调函数;

##  Request对象

```javascript
const server = http.createServer((req, res) => {
	// Request对象
	console.log(req.url)
	console.log(req.method)
	console.log(req.headers)
	res.end("Hello World!")
})
```

**Postman**测试接口

![](assets/imgs/http-req.png)

打印结果：

```
/api/test?name=aaz
GET
{
  'user-agent': 'PostmanRuntime/7.28.4',
  accept: '*/*',
  'postman-token': '6b0cc628-c773-4fa1-9fe2-f07f74786757',
  host: 'localhost:8888',
  'accept-encoding': 'gzip, deflate, br',
  connection: 'keep-alive'
}
```

根据URL和Method来处理相应的业务逻辑：

```javascript
const server = http.createServer((req, res) => {
	// Request对象
	if (req.url === "/api/test") {
		res.end("Test API!")
	}
	res.end("Hello World!")
})
```

## Response响应结果

### 返回响应结果

- `write`方法: 这种方式是直接写出数据，但是并没有关闭流; 
- `end`方法: 这种方式是写出最后的数据，并且写出后会关闭流;

```javascript
const server = http.createServer((req, res) => {
	res.write("Hello World!")
	res.end()
})
```

> 如果不调用`end`方法，客户端将一直处于等待状态直到超时。

### 设置StatusCode状态码

设置状态码的两种方式：

1. `statusCode`属性

```javascript
const server = http.createServer((req, res) => {
	res.statusCode = 201
	res.end("Hello World!")
})
```

2. `setHeader`方法

```javascript
const server = http.createServer((req, res) => {
	res.setHeader(201)
	res.end("Hello World!")
})
```

### 设置响应头

1. `setHeader`添加响应头选项

```javascript
const server = http.createServer((req, res) => {
	res.setHeader("Content-Type", "application/json;charset=utf8")
	res.end(JSON.stringify({name: "NaNa", age: 18}))
})
```

2. `writeHead`设置状态码和响应头选项

```javascript
const server = http.createServer((req, res) => {
	res.writeHead(201, {
	"Content-Type": "application/json;charset=utf8"
	})
	res.end(JSON.stringify({name: "NaNa", age: 18}))
})
```










