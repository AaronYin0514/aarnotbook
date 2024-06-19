---
tags: react react-mini react源码
---

# 实现useReducer

## Fiber

**src/ReactFiber.js**

```diff
export default function createFiber(vnode, returnFiber) {
	const fiber = {
		...
+		// 函数组件存的是hook0
+		memorizedState: null,
	};
	...
}
```

## Hooks

创建**src/hook.js**

```javascript
import { scheduleUpdateOnFiber } from "./ReactFiberWorkLoop";

let currentlyRenderingFiber = null;
let workInProgressHook = null;

export function renderWithHooks(wip) {
	currentlyRenderingFiber = wip;
	currentlyRenderingFiber.memorizedState = null;
	workInProgressHook = null;
}

function updateWorkInProgressHook() {
	let hook;

	const current = currentlyRenderingFiber.alternate;

	if (current) {
		// 组件更新
		currentlyRenderingFiber.memorizedState = current.memorizedState;
		if (workInProgressHook) {
			workInProgressHook = hook = workInProgressHook.next;
		} else {
			workInProgressHook = hook = currentlyRenderingFiber.memorizedState;
		}
	} else {
		// 组件初次渲染
		hook = {
			memorizedState: null, // state effect
			next: null, // 下一个hook
		}

		if (workInProgressHook) {
			workInProgressHook = workInProgressHook.next = hook;
		} else {
			// hook0
			workInProgressHook = currentlyRenderingFiber.memorizedState = hook;
		}
	}
	return hook;
}

export function useReducer(reducer, initalState) {
	let hook = updateWorkInProgressHook();

	if (!currentlyRenderingFiber.alternate) {
		// 初次渲染
		hook.memorizedState = initalState;
	}

	const dispatch = () => {
		console.log('log')
		hook.memorizedState = reducer(hook.memorizedState);
		currentlyRenderingFiber.alternate = { ...currentlyRenderingFiber };
		scheduleUpdateOnFiber(currentlyRenderingFiber);
	}

	return [hook.memorizedState, dispatch]
}
```

## Reconciler

```diff
-import { isArray, isStringOrNumber, updateNode } from "./utils";
+import { Update, isArray, isStringOrNumber, updateNode } from "./utils";
+import {renderWithHooks} from './hooks'

...

export function updateHostComponent(wip) {
	...
-    updateNode(wip.stateNode, wip.props);
+    updateNode(wip.stateNode, {}, wip.props);
	}
   ...
}

...

export function updateFunctionComponent(wip) {
+	renderWithHooks(wip);
...
}

// 协调（diff）
function reconcileChildren(wip, children) {
	...
+	// oldfiber的头结点
+	let oldFiber = wip.alternate?.child;
	...
+	const same = sameNode(oldFiber, newFiber);
+	
+	if (same) {
+		Object.assign(newFiber, {
+			stateNode: oldFiber.stateNode,
+			alternate: oldFiber,
+			flags: Update
+		})
+	}
+	
+	if (oldFiber) {
+		oldFiber = oldFiber.sibling;
+	}
	...
}

+// 节点复用的条件：1. 同一层级下 2. 类型相同 3. key相同
+function sameNode(a, b) {
+  return a && b && a.type === b.type && a.key === b.key;
+}
```

## WorkLoop

**src/utils.js**

```diff
-export function updateNode(node, nextVal) {
+export function updateNode(node, prevVal, nextVal) {
+	Object.keys(prevVal).forEach(k => {
+		if (k === 'children') {
+			if (isStringOrNumber(nextVal[k])) {
+			node.textContent = nextVal[k] + "";
+			}
+			} else if (k.slice(0, 2) === 'on') {
+			const eventName = k.slice(2).toLocaleLowerCase();
+			node.removeEventListener(eventName, prevVal[k]);
+		} else {
+			if (!(k in nextVal)) {
+			node[k] = '';
+			}
+		}
+	})
+
	Object.keys(nextVal).forEach((k) => {
		if (k === "children") {
			if (isStringOrNumber(nextVal[k])) {
				node.textContent = nextVal[k] + "";
			}
+		} else if (k.slice(0, 2) === 'on') {
+			const eventName = k.slice(2).toLocaleLowerCase();
+			node.addEventListener(eventName, nextVal[k]);
		} else {
			node[k] = nextVal[k];
		}
	});
}
```

### 更新节点

**ReactFiberWorkLoop.js**

```diff
-import { Placement } from "./utils";
+import { Placement, Update, updateNode } from "./utils";

function commitWorker(wip) {
	...
	if (flags & Placement && stateNode) {
		parentNode.appendChild(stateNode);
	}

+	if (flags & Update && stateNode) {
+		// 更新属性
+		updateNode(stateNode, wip.alternate.props, wip.props);
+	}
	...
}
```

## 导出

**src/react.js**

```javascript
import Component from "./Component";
import {useReducer,} from './hooks'

export { Component, useReducer };
```

## 测试

**demo/which-react.js**

```javascript
import ReactDOM from "../src/react-dom";
import { Component, useReducer } from "../src/react";

export { ReactDOM, Component, useReducer };
```

**demo/src/main.js**

```javascript
function FunctionComponent(props) {
	const [count, setCount] = useReducer((x) => x + 1, 0);
	
	return (
		<div className="border">
			<p>{props.name}</p>
			<button onClick={() => setCount()}>{count}</button>
		</div>
	);
}
```





