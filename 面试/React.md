---
tags: 
---
# React

## 状态管理

### 1 如何理解React中的state(状态)与props(属性)

> React UI = fn(state)
> 
> fn(x)  = x+1

- `state`是变量，一般情况下，`state`改变之后，组件会更新。
- `props`是属性，用于父子通信，且`props`不可修改。

### 2 什么是HOC/如何”修改“组件属性props

`props`是属性，用于父子通信，且`props`不可修改。

如果对于组件的`props`不满意，可以使用**HOC**返回一个新的组件。

HOC：高阶组件，它是个函数，接收组件作为参数，返回一个新的组件，是**Hooks**出现之前，常见的复用逻辑的手段。

如react-reudx的connect、router5的withRouter、AntD3的create等。

### 3 在函数/类组件中如何使用state

- 组件内部`state`，适合只在本组件内部使用的`state`，优点就是灵活，随时定义即可用。缺点是难以实现在组件间共享。

  函数组件内部`state`可以使用`useState`、`useReducer`定义。

  类组件组件内部可以使用`this.state`定义，使用`this.setState`修改`state`。

- 组件外部`state`，也就是所谓的状态管理库。目前用的比较多的是**Redux**、**MobX**，**DVA/Umi**也算，但是**DVA/Umi**是在**Redux**的基础上封装的，**AntD4/5 Form**也是自己在外部定义的状态管理。

### 4 Redux工作原理



  