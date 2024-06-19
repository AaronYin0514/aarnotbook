---
tags: react
---

# react优化

## 原则：将变的部分和不变的部分分离

1. `props`
2. `state`
3. `contenxt`

### Demo1

优化前：

需要优化点：`input`每次输入，`ExpensiveCpn`都会渲染

```javascript
import {useState} from 'react';

function ExpensiveCpn() {
	let now = performance.now();
	while (performance.now() - now < 100) {}
	console.log('耗时的组件 render');
	return <p>耗时的组件</p>;
}

export default function App() {
	const [num, updateNum] = useState(0);
	return (
		<div>
			<input value={num} onChange={(e) => updateNum(+e.target.value)} />
			<p>num is {num}</p>
			<ExpensiveCpn />
		</div>
	);
}
```

优化后：

优化方式：创建`Input`组件，**将变的部分和不变的部分分离**

结果：`input`每次输入，`ExpensiveCpn`都**不会**渲染

```javascript
import {useState} from 'react';

function ExpensiveCpn() {
	let now = performance.now();
	while (performance.now() - now < 100) {}
	console.log('耗时的组件 render');
	return <p>耗时的组件</p>;
}

function Input() {
	const [num, updateNum] = useState(0);
	return (
		<>
			<input value={num} onChange={(e) => updateNum(+e.target.value)} />
			<p>num is {num}</p>
		</>
	)
}

export default function App() {
	return (
		<div>
			<Input />
			<ExpensiveCpn />
		</div>
	);
}
```

### Demo2

优化前：

与上一个例子不同点：父组件`<div title={num + ""}>`使用了`state`

需要优化点：`input`每次输入，`ExpensiveCpn`都会渲染

```javascript
import {useState} from 'react';

function ExpensiveCpn() {
	let now = performance.now();
	while (performance.now() - now < 100) {}
	console.log('耗时的组件 render');
	return <p>耗时的组件</p>;
}

export default function App() {
	const [num, updateNum] = useState(0);
	return (
		<div title={num + ""}>
			<input value={num} onChange={(e) => updateNum(+e.target.value)} />
			<p>num is {num}</p>
			<ExpensiveCpn />
		</div>
	);
}
```

优化后：

优化方式：创建`InputWrapper`讲`input`和`InputWrapper`包裹，从而**将变的部分和不变的部分分离**

结果：`input`每次输入，`ExpensiveCpn`都**不会**渲染

```javascript
import {useState} from 'react';

function ExpensiveCpn() {
	let now = performance.now();
	while (performance.now() - now < 100) {}
	console.log('耗时的组件 render');
	return <p>耗时的组件</p>;
}

function InputWrapper({ children }: { children: React.ReactNode }) {
	const [num, updateNum] = useState(0);
	return (
		<div title={num + ""}>
			<input value={num} onChange={(e) => updateNum(+e.target.value)} />
			<p>num is {num}</p>
			{children}
		</div>
	);
}

export default function App() {
	return (
		<InputWrapper>
			<ExpensiveCpn />
		</InputWrapper>
	);
}
```

### 结论

> 当**父组件**满足性能优化条件，**子孙组件可能**命中性能优化

从上往下包括： 根节点，`App`，`ExpensiveCpn`。其中根节点（即`React.createRoot`创建的节点）是默认满足性能优化条件（因为根节点不会变），所以根节点的子节点（即`App`）满足`props`全等。 `App`本身又满足`state`、`context`不变，所以App满足性能优化条件，所以`App`的子节点`ExpensiveCpn`满足`props`全等，`ExpensiveCpn`本身又满足`state`、`context`不变，所以这条链路都命中性能优化

## 如何使用性能优化API

### 该如何比较props？

* 全等比较——高效，但不易命中
* 浅比较——不高效，但易命中

> ⚠️ `React`默认使用**全等**比较

### Demo1

**说明：**

1. `const [num, updateNum] = useState(0);` `num`值是变的，`updateNum`是`React`中的`Dispatch`函数，是不变的
2. `Middle`组件是不变的
3. `Button`组件是不变的
4. `Show`组件是变的
5. 由于`App`不满足优化条件，所以子组件也不满足优化条件

**优化点**：Button组件每次都会刷新

```javascript
import React, {useState, useContext} from 'react';

const numCtx = React.createContext<number>(0);
const updateNumCtx = React.createContext<React.Dispatch<number>>(() => {});

function Button() {
	const updateNum = useContext(updateNumCtx);
	console.log('btn render')
	return (
		<button onClick={() => updateNum(Math.random())}>产生随机数</button>
	)
}

function Show() {
	const num = useContext(numCtx);
	return <p>num is: {num}</p>;
}

const Middle = () => {
	return (
		<>
			<Button />
			<Show />
		</>
	)
}

export default function App() {
	const [num, updateNum] = useState(0);
	return (
		<numCtx.Provider value={num}>
			<updateNumCtx.Provider value={updateNum}>
				<Middle />
			</updateNumCtx.Provider>
		</numCtx.Provider>
	);
}
```

**优化方法**：对`Middle`函数使用`React.memo`

```javascript
const Middle = React.memo(() => {
	return (
		<>
			<Button />
			<Show />
		</>
	)
})
```

**优化说明**

1. `React`默认使用全等比较
2. 当没有传`props`时，实际传入的是`{}`，所以`Middle`和`Button`每次会更新
3. 使用`React.memo`函数，`props`比较从**全等**改为**浅比较**，使得`Middle`满足了优化条件，子组件`Button`也满足了优化条件

## 性能优化方法

1. 寻找项目中的性能损耗严重的**子树**
2. 在子树的**根节点**使用性能优化API
3. 子树中运用**变与不变分离**原则