---
tags: react react-mini react源码
---

# 实现useState

## dispatch方法

```diff
export function useReducer(reducer, initalState) {
	...
+	const dispatch = dispatchReducerAction.bind(
+		null,
+		currentlyRenderingFiber,
+		hook,
+		reducer
+	);
	return [hook.memorizedState, dispatch];
}

+function dispatchReducerAction(fiber, hook, reducer, action) {
+	hook.memorizedState = reducer ? reducer(hook.memorizedState) : action;
+	fiber.alternate = { ...fiber };
+	fiber.sibling = null;
+	scheduleUpdateOnFiber(fiber);
+}
```

## useState实现

```javascript
export function useState(initalState) {
	return useReducer(null, initalState);
}
```
