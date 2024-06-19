---
tags: react react-mini react源码
---

# 实现原生节点初次渲染

## Reconciler

**src/ReactFiberReconciler.js**

```javascript
import createFiber from "./ReactFiber";
import { isArray, isStringOrNumber, updateNode } from "./utils";

export function updateHostComponent(wip) {
  if (!wip.stateNode) {
    wip.stateNode = document.createElement(wip.type);
    // 属性
    updateNode(wip.stateNode, wip.props);
  }
  // 子节点
  reconcileChildren(wip, wip.props.children);
}
 
function reconcileChildren(wip, children) {
  if (isStringOrNumber(children)) {
    return;
  }
  const newChildren = isArray(children) ? children : [children];
  let previousNewFiber = null; // 记录上一次的fiber
  for (let i = 0; i < newChildren.length; i++) {
    const newChild = newChildren[i];
    const newFiber = createFiber(newChild, wip);

    if (i === 0) {
      wip.child = newFiber;
    } else {
      previousNewFiber.sibling = newFiber;
    }

    previousNewFiber = newFiber;
  }
}
```

**src/utils.js**

```javascript
export function updateNode(node, nextVal) {
  Object.keys(nextVal).forEach(k => {
    if (k === 'children') {
      if (isStringOrNumber(nextVal[k])) {
        node.textContent = nextVal[k] + '';
      }
    } else {
      node[k] = nextVal[k];
    }
  })
}
```

## WookLoop

**src/ReactFiberWorkLoop.js**

```javascript
import { Placement, isFn, isStr } from "./utils";

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

  let parentNode = wip.return.stateNode;
  if (flags && Placement && stateNode) {
    parentNode.appendChild(stateNode);
  }

  // 2 更新子节点
  commitWorker(wip.child);
  // 3 更新兄弟节点
  commitWorker(wip.sibling);
}
```


