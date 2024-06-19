---
tags: javascript, js, react
---

# 组件实例3大属性-props

## props

**理解**

1. 每个组件对象都会有`props`(properties的简写)属性
2. 组件标签的所有属性都保存在`props`中

**作用**

1.  通过标签属性从组件外向组件内传递变化的数据
2.  注意: 组件内部不要修改props数据

## 基本使用

```javascript
//创建组件
class Person extends React.Component{
	render(){
		const {name,age,sex} = this.props
		return (
			<ul>
				<li>姓名：{name}</li>
				<li>性别：{sex}</li>
				<li>年龄：{age+1}</li>
			</ul>
		)
	}
}

//渲染组件到页面
ReactDOM.render(<Person name="jerry" age={19} sex="男"/>,document.getElementById('test1'))
ReactDOM.render(<Person name="tom" age={18} sex="女"/>,document.getElementById('test2'))

const p = {name:'老刘',age:18,sex:'女'}
ReactDOM.render(<Person {...p}/>,document.getElementById('test3'))
```

## 对props类型限制

```javascript
//创建组件
class Person extends React.Component{
	render(){
		// console.log(this);
		const {name,age,sex} = this.props
		// props是只读的
		// this.props.name = 'jack' //此行代码会报错，因为props是只读的
		return (
			<ul>
				<li>姓名：{name}</li>
				<li>性别：{sex}</li>
				<li>年龄：{age+1}</li>
			</ul>
		)
	}
}

//对标签属性进行类型、必要性的限制
Person.propTypes = {
	name: PropTypes.string.isRequired, //限制name必传，且为字符串
	sex: PropTypes.string,//限制sex为字符串
	age: PropTypes.number,//限制age为数值
	speak: PropTypes.func,//限制speak为函数
}

//指定默认标签属性值
Person.defaultProps = {
	sex: '男',//sex默认值为男
	age: 18 //age默认值为18
}

//渲染组件到页面
ReactDOM.render(<Person name={100} speak={speak}/>,document.getElementById('test1'))
ReactDOM.render(<Person name="tom" age={18} sex="女"/>,document.getElementById('test2'))
```

## props简写方式

```javascript
//创建组件

class Person extends React.Component{
	constructor(props){
		//构造器是否接收props，是否传递给super，取决于：
		// 是否希望在构造器中通过this访问props
		// console.log(props);
		super(props)
		console.log('constructor', this.props);
	}

	//对标签属性进行类型、必要性的限制
	static propTypes = {
		name: PropTypes.string.isRequired, //限制name必传，且为字符串
		sex: PropTypes.string,//限制sex为字符串
		age: PropTypes.number,//限制age为数值
	}

	//指定默认标签属性值
	static defaultProps = {
		sex: '男',//sex默认值为男
		age: 18 //age默认值为18
	}

	render(){
		// console.log(this);
		const {name,age,sex} = this.props
		//props是只读的
		//this.props.name = 'jack' //此行代码会报错，因为props是只读的
		return (
			<ul>
			<li>姓名：{name}</li>
			<li>性别：{sex}</li>
			<li>年龄：{age+1}</li>
			</ul>
		)
	}
}

//渲染组件到页面
ReactDOM.render(<Person name="jerry"/>,document.getElementById('test1'))
```















