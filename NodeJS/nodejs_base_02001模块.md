---
tags: [nodejs]
---

# 模块

## 导出模块

## exports

`exports`是一个对象，将要导出的对象添加到该对象。在其它文件中，通过`require`就可以导入使用了。

foo.js

```javascript
const name = "JavaScript"
const age = 100
const sayHello = () => {
	console.log("Hello,JavaScript!")
}

exports.name = name
exports.age = age
exports.sayHello = sayHello
```

bar.js

```javascript
const foo = require("./foo")

console.log(foo.name)
console.log(foo.age)
foo.sayHello()
```

执行：

```shell
node bar.js
```

`foo`是对`exports`的浅层拷贝。

## module.exports

module对象的exports属性是exports对象的一个引用。所以`module.exports` = `exports`

## 导入模块

## require

`require`是一个函数，可以引用一个文件（模块）中导出的对象。

### require路径查找顺序

以`require(X)`为例：

**情况一**: X是一个核心模块，比如path、http p 直接返回核心模块，并且停止查找

**情况二**: X是以`./`或`../`或`/`(根目录)开头的

第一步: 将X当做一个文件在对应的目录下查找;  

- 如果有后缀名，按照后缀名的格式查找对应的文件
- 如果没有后缀名，会按照如下顺序:

	1. 直接查找文件X 
	2. 查找X.js文件  
	3. 查找X.json文件
	4. 查找X.node文件

- 如果没有找到，那么报错:not found

第二步:没有找到对应的文件，将X作为一个目录 p 查找目录下面的index文件

1. 查找`X/index.js`文件  
2. 查找`X/index.json`文件 
3. 查找`X/index.node`文件

**情况三**:直接是一个X(没有路径)，并且X不是一个核心模块

- 在`module.paths`的目录列表中查找

```
[
'/Users/xxxx/Documents/Test/Test/node_modules',
'/Users/xxxx/Documents/Test/node_modules',
'/Users/xxxx/Documents/node_modules',
'/Users/xxxx/node_modules',
'/Users/node_modules',
'/node_modules'
]
```

- 如果上面的路径中都没有找到，那么报错:not found

## 模块的加载过程

**结论一**：模块在被第一次引入时，模块中的js代码会被运行一次

**结论二**：模块被多次引入时，会缓存，最终只加载（运行）一次

		每个模块对象`module`都有一个属性：`loaded`。为`false`表示还没有加载，为`true`表示已经加载。

**结论三**：如果有循环引入，那么加载顺序是什么？

	模块间的引用关系对应图的数据结构，图结构在遍历的过程中，有深度优先搜索（DFS, depth first search）和广度优先搜索（BFS, breadth first search）。Node采用的是深度优先算法。
	


