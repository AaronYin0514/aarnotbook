---
tags: [nodejs]
---

# events模块

Node中的核心API都是基于异步事件驱动的:  在这个体系中，某些对象(发射器(Emitters))发出某一个事件;我们可以监听这个事件(监听器 Listeners)，并且传入的回 调函数，这个回调函数会在监听到事件时调用。

## 事件监听

发出事件和监听事件都是通过EventEmitter类来完成的，它们都属 于events对象。

API：

```javascript
emitter.on(eventName, listener) //监听事件，也可以使用 addListener;

emitter.off(eventName, listener) //移除事件监听，也可以使 用removeListener;

emitter.emit(eventName[, ...args]) //发出事件，可以携带一 些参数;
```

事件监听：

```javascript
const EventEmmiter = require("events")

const bus = new EventEmmiter()

function clickHandle(...args) {
	console.log("监听到click事件", args)
}

bus.on("click", clickHandle)

setTimeout(() => {
	bus.emit("click", "a", "b")
	bus.off("click", clickHandle)
	bus.emit("click", "a")
})
```

## 常见属性

- **emitter.eventNames()**:返回当前 EventEmitter对象注册的事件字符串数组;
- **emitter.getMaxListeners()**:返回当前 EventEmitter对象的最大监听器数量，可以通过setMaxListeners() 来修改，默认是10;
- **emitter.listenerCount(事件名称)**:返回当前 EventEmitter对象某一个事件名称，监听器的个数; 
- **emitter.listeners(事件名称)**:返回当前 EventEmitter对象某个事件监听器上所有的监听器数组;

## 事件只监听一次

```javascript
//  emitter.once(eventName, listener)
bus.once("click", (args) => {
	console.log("监听到click事件", args)
})
```

## 将监听事件添加到最前面

```javascript
// emitter.prependListener()
bus.prependListener("click", args => {
	console.log("监听到click事件", args)
})
```

## 将监听事件添加到最前面，且只监听一次

```javascript
// emitter.prependOnceListener()
bus.prependOnceListener("click", args => {
	console.log("监听到click事件", args)
})
```

## 移除所有监听器

```javascript
// emitter.removeAllListeners([eventName])
bus.removeAllListeners()
bus.removeAllListeners("click")
```

