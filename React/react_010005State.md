---
tags: javascript, js, react
---

# 组件实例3大属性-state

 ## state
 
1. `state`是组件对象最重要的属性, 值是对象(可以包含多个key-value的组合)
2. 组件被称为"**状态机**", 通过更新组件的`state`来更新对应的页面显示(重新渲染组件)

## 基本使用

```javascript
//1.创建组件
class Weather extends React.Component{
	//构造器调用几次？ ———— 1次
	constructor(props){
		super(props)
		//初始化状态
		this.state = {isHot:false,wind:'微风'}
		//解决changeWeather中this指向问题
		this.changeWeather = this.changeWeather.bind(this)
	}

	changeWeather(){
		//获取原来的isHot值
		const isHot = this.state.isHot
		//严重注意：状态必须通过setState进行更新,且更新是一种合并，不是替换。
		this.setState({isHot:!isHot})
	}

	//render调用几次？ ———— 1+n次 1是初始化的那次 n是状态更新的次数
	render(){
		//读取状态
		const {isHot,wind} = this.state
		return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}，{wind}</h1>
	}
}

//2.渲染组件到页面
ReactDOM.render(<Weather/>,document.getElementById('test'))
```

## 简写方式

```javascript
//1.创建组件
class Weather extends React.Component{
	//初始化状态
	state = {isHot:false,wind:'微风'}

	render() {
		const {isHot,wind} = this.state
		return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}，{wind}</h1>
	}

	//自定义方法————要用赋值语句的形式+箭头函数
	changeWeather = ()=>{
		const isHot = this.state.isHot
		this.setState({isHot:!isHot})
	}
}

//2.渲染组件到页面
ReactDOM.render(<Weather/>,document.getElementById('test'))
```

## ⚠️注意

**1 状态必须通过`setState`进行更新,且更新是一种合并，不是替换。**

```javascript
// ✅ 严重注意：状态必须通过setState进行更新,且更新是一种合并，不是替换。
this.setState({isHot:!isHot})

// ❌ 严重注意：状态(state)不可直接更改，下面这行就是直接更改
this.state.isHot = !isHot
```

**2 类对象方法中this问题**

1. 组件中render方法中的this为组件实例对象
2. 组件自定义的方法中this为undefined，如何解决？
	1. 强制绑定this: 通过函数对象的bind()
	2. 箭头函数

> 类中的方法默认开启了局部的严格模式，所以changeWeather中的this为undefined
> 非严格模式，this是window

```javascript
// ✅ 1 bind强制绑定
constructor(props){
	super(props)
	//初始化状态
	this.state = {isHot:false,wind:'微风'}
	//解决changeWeather中this指向问题
	this.changeWeather = this.changeWeather.bind(this)
}

// ✅ 2 使用箭头函数
changeWeather = ()=>{
	const isHot = this.state.isHot
	this.setState({isHot:!isHot})
}
```

## setState

```javascript
class Demo extends Component {

	state = {count:0}
	
	add = () => {
		//对象式的setState
		
		// 1.获取原来的count值
		const {count} = this.state
		
		// 2.更新状态
		this.setState({count:count+1}, ()=>{
			console.log(this.state.count);
		})
		//console.log('12行的输出',this.state.count); //0 
		
		//函数式的setState
		this.setState( state => ({count:state.count+1}))
	}

	render() {
		return (
			<div>
				<h1>当前求和为：{this.state.count}</h1>
				<button onClick={this.add}>点我+1</button>
			</div>
		)
	}
}
```























