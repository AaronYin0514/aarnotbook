---
tags: react react-mini react源码
---

# 实现函数组件、类组件、文本、Fragment的初次渲染

## Fiber

**src/ReactFiber.js**

```javascript
import {
	ClassComponent,
	FunctionComponent,
	HostComponent,
	HostText,
	Fragment,
} from "./ReactWorkTags";
import { Placement, isFn, isStr, isUndefined } from "./utils";

export default function createFiber(vnode, returnFiber) {
	const fiber = {
		type: vnode.type,
		key: vnode.key,
		props: vnode.props,
		stateNode: null, // 原生标签时候指dom节点，类组件时候指的是实例
		child: null, // 第一个子fiber
		sibling: null, // 下一个兄弟fiber
		return: returnFiber, // 父fiber
		flags: Placement,
		// 老节点
		alternate: null,
		deletions: null, // 要删除子节点 null或者[]
		index: null, //当前层级下的下标，从0开始
	};
	
	const { type } = vnode;
	
	if (isStr(type)) {
		fiber.tag = HostComponent;
	} else if (isFn(type)) {
		fiber.tag = type.prototype.isReactComponent
		? ClassComponent
		: FunctionComponent;
	} else if (isUndefined(type)) {
		fiber.tag = HostText;
		fiber.props = { children: vnode };
	} else {
		fiber.tag = Fragment;
	}
	
	return fiber;
}
```

## WorkLoop

```javascript
import {
	updateClassComponent,
	updateFragmentComponent,
	updateFunctionComponent,
	updateHostComponent,
	updateHostTextComponent,
} from "./ReactFiberReconciler";
import {
	ClassComponent,
	Fragment,
	FunctionComponent,
	HostComponent,
	HostText,
} from "./ReactWorkTags";
import { Placement, isFn, isStr } from "./utils";

let wip = null; // work in progress
let wipRoot = null;

export function scheduleUpdateOnFiber(fiber) {
	wip = fiber;
	wipRoot = fiber;
}

export function performUnitWork() {
	// todo 1. 执行当前任务wip
	// 判断wip是什么类型的组件
	console.log(wip);
	const { tag } = wip;
	switch (tag) {
		case HostComponent:
			// 原生标签
			updateHostComponent(wip);
			break;
		case FunctionComponent:
			updateFunctionComponent(wip);
			break;
		case ClassComponent:
			updateClassComponent(wip);
			break;
		case Fragment:
			updateFragmentComponent(wip);
			break;
		case HostText:
			updateHostTextComponent(wip);
			break;
		default:
			break;
	}

	// todo 2. 更新wip
	// 深度优先遍历
	if (wip.child) {
		wip = wip.child;
		return;
	}

	let next = wip;
	while (next) {
		if (next.sibling) {
			wip = next.sibling;
			return;
		}
		next = next.return;
	}

	wip = null;
}

function workLoop(IdleDeadline) {
	while (wip && IdleDeadline.timeRemaining() > 0) {
		performUnitWork();
	}

	if (!wip && wipRoot) {
		commitRoot();
	}
}

requestIdleCallback(workLoop);

function commitRoot() {
	commitWorker(wipRoot);
	wipRoot = null;
}

function commitWorker(wip) {
	if (!wip) return;
	// 1 更新自己
	const { flags, stateNode } = wip;
	let parentNode = getParentNode(wip.return);
	if (flags & Placement && stateNode) {
		parentNode.appendChild(stateNode);
	}
	
	// 2 更新子节点
	commitWorker(wip.child);
	// 3 更新兄弟节点
	commitWorker(wip.sibling);
}

function getParentNode(wip) {
	let tem = wip;
	while (tem) {
		if (tem.stateNode) {
			return tem.stateNode;
		}
		
		tem = tem.return;
	}
}
```

## Reconciler

### 协调

**src/ReactFiberReconciler.js**

```javascript
// 协调（diff）
function reconcileChildren(wip, children) {
	if (isStringOrNumber(children)) {
		return;
	}

	const newChildren = isArray(children) ? children : [children];
	let previousNewFiber = null; // 记录上一次的fiber
	
	for (let i = 0; i < newChildren.length; i++) {
		const newChild = newChildren[i];
		if (newChild == null) {
			continue;
		}

		const newFiber = createFiber(newChild, wip);
		if (previousNewFiber === null) {
			// head node
			wip.child = newFiber;
		} else {
			previousNewFiber.sibling = newFiber;
		}
		previousNewFiber = newFiber;
	}
}
```

### 函数式组件

**src/ReactFiberReconciler.js**

```javascript
export function updateFunctionComponent(wip) {
	const { type, props } = wip;
	const children = type(props);
	reconcileChildren(wip, children);
}
```

### 类组件

创建Component，**src/Component.js**

```javascript
export default function Component(props) {
	this.props = props;
}

Component.prototype.isReactComponent = {};
```

**src/ReactFiberReconciler.js**

```javascript
export function updateClassComponent(wip) {
	const { type, props } = wip;
	const instance = new type(props);
	const children = instance.render();
	reconcileChildren(wip, children);
}
```

### 文本

```javascript
export function updateHostTextComponent(wip) {
	wip.stateNode = document.createTextNode(wip.props.children);
}
```


### Fragment

```javascript
export function updateFragmentComponent(wip) {
	reconcileChildren(wip, wip.props.children);
}
```

## 导出

创建**src/react.js**

```javascript
import Component from "./Component";
export { Component };
```

## 测试

**demo/which-react.js**

```javascript
import ReactDOM from "../src/react-dom";
import { Component } from "../src/react";

export { ReactDOM, Component };
```

**demo/src/main.js**

```javascript
import { ReactDOM, Component } from "../which-react";
import "./index.css";

function FunctionComponent(props) {
	return (
		<div className="border">
			<p>{props.name}</p>
		</div>
	);
}

class ClassComponent extends Component {
	render() {
		return (
			<div>
				<h3>{this.props.name}</h3>
				我是文本
			</div>
		);
	}
}

function FragmentComponent() {
	return (
		<ul>
			<>
				<li>part1</li>
				<li>part2</li>
			</>
		</ul>
	);
}

const jsx = (
	<div className="border">
		<h1>react1</h1>
		<a href="https://github.com/bubucuo/mini-react">mini react</a>
		<FunctionComponent name="函数组件" />
		<ClassComponent name="类组件" />
		<FragmentComponent />
	</div>
);

ReactDOM.createRoot(document.getElementById("root")).render(jsx);
```
