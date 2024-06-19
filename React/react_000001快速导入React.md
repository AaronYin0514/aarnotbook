---
tags: javascript, js, react
---

# 快速导入React—— Hello,React

## 代码

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>hello_react</title>
</head>
<body>
  <!-- 准备好一个“容器” -->
  <div id="test"></div>

  <!-- 引入react核心库 -->
  <script type="text/javascript" src="https://unpkg.com/react@17/umd/react.development.js"></script>
  <!-- 引入react-dom，用于支持react操作DOM -->
  <script type="text/javascript" src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
  <!-- 引入babel，用于将jsx转为js -->
  <script type="text/javascript" src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>

  <script type="text/babel" > /* 此处一定要写babel */
    //1.创建虚拟DOM
    const VDOM = <h1>Hello,React</h1> /* 此处一定不要写引号，因为不是字符串 */
    //2.渲染虚拟DOM到页面
    ReactDOM.render(VDOM,document.getElementById('test'))
  </script>
</body>
</html>
```

## React库介绍

```javascript
  <script type="text/javascript" src="https://unpkg.com/react@17/umd/react.development.js"></script>
  <script type="text/javascript" src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
  <script type="text/javascript" src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

* react: React核心库
* react-dom: 用于支持React操作DOM
* babel: 将JSX语法翻译为JS

## 创建过程

### 1. 代码

```javascript
  <script type="text/babel" >
    const VDOM = <h1>Hello,React</h1>
    ReactDOM.render(VDOM,document.getElementById('test'))
  </script>
```

### 2. 流程

1. 创建虚拟DOM`const VDOM = <h1>Hello,React</h1>`
2. 渲染虚拟DOM到页面`ReactDOM.render(VDOM,document.getElementById('test')`

### 3. 注意事项

1. `<script type="text/babel" >`一定是**babel**，而不是javascript。因为脚本内使用的**JSX**语法，而不是JS
2. 同样道理，`<h1>Hello,React</h1>`没有引号
3. 使用React尽量不要直接操作真实DOM，`ReactDOM.render(VDOM,document.getElementById('test')`最好只有这一处直接操作dom

## 3. 虚拟DOM

React提供了一些API，用来创建虚拟DOM，**DOM Diffing**算法可以最小化页面重绘。所以React更加高效。

虚拟DOM最终会被转化为真实DOM，渲染到页面上。

### 使用JS创建虚拟DOM

```javascript
//1.创建虚拟DOM
const VDOM = React.createElement('h1',{id:'title'},React.createElement('span',{},'Hello,React'))
//2.渲染虚拟DOM到页面
ReactDOM.render(VDOM,document.getElementById('test'))
```

### 使用jsx创建虚拟DOM

```javascript
//1.创建虚拟DOM
const VDOM = ( /* 此处一定不要写引号，因为不是字符串 */
	<h1 id="title">
		<span>Hello,React</span>
	</h1>
)

//2.渲染虚拟DOM到页面
ReactDOM.render(VDOM,document.getElementById('test'))
```

## 4. JSX

可以看出，创建虚拟DOM即可以使用JS创建（React提供的API），也可以使用JSX方式。

事实上，JSX是React提供的API的语法糖，最终通过babel库将JSX语法翻译为JS语法，才能被浏览器识别。