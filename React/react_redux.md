---
tags: [react]
---

# React-redux

react提供了非常好用的react hook工程，管理组件的状态和显示。而对于复杂的组件多层级间依赖，面临着如何简洁的实现组件间状态共享的难题，redux可以帮助我们解决这样的问题。

## Redux原理

![](assets/imgs/react/redux.png)

redux三个核心概念：action、reducer、store。

- action：是一个对象，包含两个属性type和data。type是一字符串，作为action的标识。data是任意数据类型。
- reducer：用于初始化状态和加工状态的[[编程概念##纯函数|纯函数]]。reducer通过store接收action，根据type对应不同的逻辑处理，data作为参数，返回新的状态。
- store：将state、action和reducer联系在一起的对象。

## 创建react项目

```shell
create-react-app projectname
cd projectname
```

## 添加redux依赖

```shell
yarn add redux
yarn add redux-thrunk
yarn add react-redux
```

- redux: 集中式管理应用中多组件共享状态
- redux-thrunk: 异步中间件
- react-redux: 链接redux的状态和react的组件

## 创建一个Count组件

对于一个简单的组件，我们可以使用`useState`维护自己的状态，而redux是将状态集中管理。

`Count`组件只负责页面的渲染和交互，而是将状态通过外界传入再显示，将交互的实现也同样抛给外界实现。

`Count`组件包含两个状态，`count`表示当前的总和，`persons`表示一个存放person对象的数组。

`Count`组件包含3个action函数，`increment`表示加法，`decrement`表示减法，`incrementAsync`表示异步加法，将业务交互的实现抛给外部进行实现。

```javascript
import React, {useRef} from 'react'

function Count({
	count,
	presons,
	increment,
	decrement,
	incrementAsync
}) {
	const selectRef = useRef()
	
	const _increment = () => {
		const value = selectRef.current.value
		increment(value * 1)
	}
	
	const _decrement = () => {
		const value = selectRef.current.value
		decrement(value * 1)
	}

	const _incrementIfOdd = () => {
		const value = selectRef.current.value
		if (count % 2 !== 0) {
			increment(value * 1)
		}
	}
	
	const _incrementAsync = () => {
		const value = selectRef.current.value
		incrementAsync(value * 1, 500)
	}

	return (
	<div>
		<h2>我是Count组件,下方组件总人数为:{presons.length}</h2>
		<h4>当前求和为：{count}</h4>
		<select ref={selectRef}>
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>&nbsp;
		<button onClick={_increment}>+</button>&nbsp;
		<button onClick={_decrement}>-</button>&nbsp;
		<button onClick={_incrementIfOdd}>当前求和为奇数再加</button>&nbsp;
		<button onClick={_incrementAsync}>异步加</button>
	</div>
	)
}
```

## 创建Person组件

同样的道理，创建一个`Person`组件，最终实现与`Count`组件的状态共享。

```javascript
import React, {useRef} from 'react'
import { nanoid } from 'nanoid'

function Person({
	count,
	persons,
	addPerson
}) {
	const nameRef = useRef()
	const ageRef = useRef()

	const _addPerson = () => {
		const name = nameRef.current.value
		const age = ageRef.current.value * 1

		const personObj = {id: nanoid(), name, age}
		addPerson(personObj)
		nameRef.current.value = ""
		ageRef.current.value = ""
	}

	return (
	<div>
		<h2>我是Person组件,上方组件求和为{count}</h2>
		<input ref={nameRef} type="text" placeholder="输入名字"></input>
		<input ref={ageRef} type="text" placeholder="输入年龄"></input>
		<button onClick={_addPerson}>添加</button>
		<ul>
		{
			persons.map( p => {
				return <li key={p.id}>{p.name}--{p.age}</li>
			})
		}
		</ul>
	</div>
	)
}
```

## 应用主页App.js

应用主页将显示这两个组件组成的页面。

```javascript
import Count from './containers/Count';
import Person from './containers/Person';

function App() {
	return (
	<div>
		<Count />
		<hr />
		<Person />
	</div>
	);
}
```

## 创建Actions

前面说到将页面交互通过action抛出给外部实现业务逻辑，那么现在创建action。

创建`redux/actions/count.js`

创建`increment`和`decrement`两个函数，函数返回`action`对象（如前面所说，`action`是一个对象，包含`type`和`data`两个属性）。

`incrementAsync`在[连接state、action与组件](##连接state、action与组件)中解释。

```javascript
import { INCREMENT, DECREMENT } from '../constant'

export const increment = data => ({
	type: INCREMENT,
	data
})

export const decrement = data => ({
	type: DECREMENT,
	data
})

export const incrementAsync = (data, time) => {
	return (dispatch) => {
		setTimeout(() => {
			dispatch(increment(data))
		}, time)
	}
}
```

同理，创建`redux/actions/person.js`

```javascript
import {ADD_PERSON} from '../constant'

export const addPerson = personObj => (
	{
		type: ADD_PERSON,
		data: personObj
	}
)
```

## 连接state、action与组件

从[redux原理图中](##Redux原理)可以看出，redux提供了`dispatch`函数，将`action`传递`store`对象，再由`store`将当前`state`和`action`传给`reducer`。

而`increment`和`decrement`两个方法中并没有看到`dispatch`方法，这是因为接下来我们会通过`react-redux`库完成这项工作。

`react-redux`将state状态、action方法通过props的方式传递给组件，当组件调用action方法时，`react-redux`会帮助你调用`dispatch`方法，将`action`传递给`store`。

Count组件：

```javascript
export default connect(
	state => ({
		count: state.count,
		presons: state.persons
	}),
	{increment, decrement, incrementAsync}
)(Count)
```

Person组件:

```javascript
import {connect} from 'react-redux'
import { addPerson } from '../../redux/actions/person'

export default connect(
	state => ({
		persons: state.persons,
		count: state.count
	}),
	{addPerson}
)(Person)
```

对于异步方法`incrementAsync`，`react-redux`也不知道什么时机调用`dispatch`方法，所以将`dispatch`方法通过闭包的方式传递给了`incrementAsync`函数。

`redux`默认并不支持异步方式，它需要`redux-thunk`中间件的帮助，这需要在[创建Store](##创建Store)时配置。

## 创建Reducers

`store`将当前的状态值和`action`对象传递给`reducer`，`reducer`需要更具`action`的`type`来判断业务上怎样处理并返回新的状态值。

创建`redux/reducers/count.js`

```javascript
import { INCREMENT, DECREMENT } from '../constant'

const initState = 0

export default function countReducer(preState=initState, action) {
	const { type, data } = action
	switch (type) {
		case INCREMENT:
			return preState + data
		case DECREMENT:
			return preState - data
		default:
			return preState
	}
}
```

> 状态初始值：
>
> 在redux初始化时，store会向reducer发送类似@@...的action来初始化状态。这时没有能匹配到的action的type，所以default返回的值就是状态的初始值。

创建`redux/reducers/person.js`

```javascript
import { ADD_PERSON } from '../constant'

const initState = [{id: '001', name: 'tom', age: 18}]

export default function personReducer(preState=initState, action) {
	const {type, data} = action
	switch(type) {
		case ADD_PERSON:
			return [data, ...preState]
		default:
			return preState
	}
}
```

`INCREMENT`, `DECREMENT`和`ADD_PERSON`是三个全局变量，定义在`redux/constant.js`文件中

```javascript
export const INCREMENT = 'increment'
export const DECREMENT = 'decrement'
export const ADD_PERSON = 'add_person'
```

## 合并Reducers

创建`store`时需要将所有的`reducer`传给`store`，redux提供了`combineReducers`函数完成这个功能。

创建`redux/reducers/index.js`

```javascript
import { combineReducers } from 'redux'
import count from './count'
import persons from './person'

export default combineReducers({
	count,
	persons
})
```

## 创建Store

创建`redux/store.js`

redux提供`createStore`函数创建`store`。`redux-thunk`中间件使`redux`支持异步操作，在创建`store`时，传入中间件`applyMiddleware(thunk)`

```javascript
import {createStore, applyMiddleware} from 'redux'
import reducer from './reducers'
import thunk from 'redux-thunk'

export default createStore(
	reducer,
	applyMiddleware(thunk)
)
```

## 设置Store

我们需要将`store`对象传递给react的全部组件，react-redux提供了`Provider`组件来完成该工作。

项目index.js

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import store from './redux/store'
import {Provider} from 'react-redux'

ReactDOM.render(
	<Provider store={store}>
		<App />
	</Provider>,
document.getElementById('root')
);
```

这是可以运行项目：

```shell
yarn start
```

## Redux调试

安装调试依赖库：

```shell
yarn add redux-devtools-extension
```

在创建`store`时：

```javascript
import {createStore, applyMiddleware} from 'redux'
import reducer from './reducers'
import thunk from 'redux-thunk'
import {composeWithDevTools} from 'redux-devtools-extension'

export default createStore(
	reducer,
	composeWithDevTools(applyMiddleware(thunk))
)
```

![](assets/imgs/react/redux-debug.png)




