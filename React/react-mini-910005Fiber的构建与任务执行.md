---
tags: react react-mini react源码
---

# Fiber的构建与任务执行

## 1 组件类型

组件类型声明，包括**文本节点**、**HTML标签节点**、**函数组件**、**类组件**、**Fragment**等等。

**src/ReactWorkTags.js**

```javascript
export const FunctionComponent = 0;
export const ClassComponent = 1;
export const IndeterminateComponent = 2; // Before we know whether it is function or class
export const HostRoot = 3; // Root of a host tree. Could be nested inside another node.
export const HostPortal = 4; // A subtree. Could be an entry point to a different renderer.
export const HostComponent = 5;
export const HostText = 6;
export const Fragment = 7;
export const Mode = 8;
export const ContextConsumer = 9;
export const ContextProvider = 10;
export const ForwardRef = 11;
export const Profiler = 12;
export const SuspenseComponent = 13;
export const MemoComponent = 14;
export const SimpleMemoComponent = 15;
export const LazyComponent = 16;
export const IncompleteClassComponent = 17;
export const DehydratedFragment = 18;
export const SuspenseListComponent = 19;
export const ScopeComponent = 21;
export const OffscreenComponent = 22;
export const LegacyHiddenComponent = 23;
export const CacheComponent = 24;
```

## 2 Fiber结构

![](../assets/imgs/react/fiber.png)

**src/ReactFiber.js**

```javascript
import {Placement} from "./utils";

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
	
	return fiber;
}
```

## 3 Reconciler

**src/ReactReconciler.js**

```javascript
export function updateHostComponent() { }

export function updateFunctionComponent() { }

export function updateClassComponent() { }

export function updateFragmentComponent() { }

export function updateHostTextComponent() { }
```

## 4 WorkLoop

**src/ReactWorkLoop.js**

```javascript
import { updateClassComponent, updateFunctionComponent, updateHostComponent } from "./ReactReconciler";
import { isFn, isStr } from "./utils";

let wip = null; // work in progress

export function performUnitWork() {
	// todo 1. 执行当前任务wip
	// 判断wip是什么类型的组件
	const { type } = wip;
	
	if (isStr(type)) {
		// 原生标签
		updateHostComponent(wip)
	} else if (isFn(wip)) {
		type.prototype.isReactComponent ? updateClassComponent(wip) : updateFunctionComponent(wip);
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
```

## 工具方法

**src/utils.js**

```javascript
export const NoFlags = 0b00000000000000000000;
export const Placement = 0b0000000000000000000010; // 2
export const Update = 0b0000000000000000000100; // 4
export const Deletion = 0b0000000000000000001000; // 8

export function isStr(s) {
	return typeof s === 'string';
}

export function isStringOrNumber(s) {
	const t = typeof s;
	return t === 'string' || t === 'number';
}

export function isFn(fn) {
	return typeof fn === 'function';
}

export function isArray(arr) {
	return Array.isArray(arr);
}

export function isUndefined(un) {
  return un === undefined;
}
```


