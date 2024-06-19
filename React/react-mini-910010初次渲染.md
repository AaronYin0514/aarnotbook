---
tags: react react-mini react源码
---

# 初次渲染

## WorkLoop

**src/ReactFiberWorkLoop.js**

```javascript
let wip = null; // work in progress
let wipRoot = null;

export function scheduleUpdateOnFiber(fiber) {
	wip = fiber;
	wipRoot = fiber;
}
```

## 虚拟dom

**src/react-dom.js**

```javascript
import createFiber from "./ReactFiber";
import { scheduleUpdateOnFiber } from "./ReactFiberWorkLoop";

function ReactDOMRoot(internalRoot) {
	this._internalRoot = internalRoot;
}

ReactDOMRoot.prototype.render = function (children) {
	const root = this._internalRoot;
	updateContainer(children, root);
}

function createRoot(container) {
	const root = {
		containerInfo: container
	};
	
	return new ReactDOMRoot(root);
}

function updateContainer(element, container) {
	const { containerInfo } = container;
	const fiber = createFiber(element, {
		type: containerInfo.nodeName.toLowerCase(),
		stateNode: containerInfo
	});
	scheduleUpdateOnFiber(fiber);
}

export default {
	createRoot,
}
```

## 测试

**demo/which-react.js**

```javascript
import ReactDOM from "../src/react-dom";

export {ReactDOM};
```


