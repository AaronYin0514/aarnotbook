---
tags: javascript, js, react
---

# 组件实例3大属性-ref

## Ref

组件内的标签可以定义**ref**属性来标识自己

## 字符串形式ref

```javascript
//创建组件
class Demo extends React.Component{
	//展示左侧输入框的数据
	showData = ()=>{
		const {input1} = this.refs
		alert(input1.value)
	}
	
	//展示右侧输入框的数据
	showData2 = ()=>{
		const {input2} = this.refs
		alert(input2.value)
	}
	
	render(){
		return(
			<div>
				<input ref="input1" type="text" placeholder="点击按钮提示数据"/>&nbsp;
				<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
				<input ref="input2" onBlur={this.showData2} type="text" placeholder="失去焦点提示数据"/>
			</div>
		)
	}
}

//渲染组件到页面
ReactDOM.render(<Demo a="1" b="2"/>,document.getElementById('test'))
```

```javascript
<input ref="input1" type="text" placeholder="点击按钮提示数据"/>
```

## 回调函数形式ref

```javascript
//创建组件
class Demo extends React.Component{
	//展示左侧输入框的数据
	showData = ()=>{
		const {input1} = this
		alert(input1.value)
	}
	
	//展示右侧输入框的数据
	showData2 = ()=>{
		const {input2} = this
		alert(input2.value)
	}
	
	render(){
		return(
			<div>
				<input ref={c => this.input1 = c } type="text" placeholder="点击按钮提示数据"/>&nbsp;
				<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
				<input onBlur={this.showData2} ref={c => this.input2 = c } type="text" placeholder="失去焦点提示数据"/>&nbsp;
			</div>
		)
	}
}

//渲染组件到页面
ReactDOM.render(<Demo a="1" b="2"/>,document.getElementById('test'))
```

## createRef

`React.createRef`调用后可以返回一个容器，该容器可以存储被`ref`所标识的节点,该容器是“**专人专用**”的。

```javascript
//创建组件
class Demo extends React.Component{
	myRef = React.createRef()
	myRef2 = React.createRef()

	//展示左侧输入框的数据
	showData = ()=>{
		alert(this.myRef.current.value);
	}
	
	//展示右侧输入框的数据
	showData2 = ()=>{
		alert(this.myRef2.current.value);
	}

	render(){
		return(
			<div>
				<input ref={this.myRef} type="text" placeholder="点击按钮提示数据"/>&nbsp;
				<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
				<input onBlur={this.showData2} ref={this.myRef2} type="text" placeholder="失去焦点提示数据"/>&nbsp;
			</div>
		)
	}
}

//渲染组件到页面
ReactDOM.render(<Demo a="1" b="2"/>,document.getElementById('test'))
```