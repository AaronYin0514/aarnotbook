---
tags: javascript, js, react
---

# 快速入门JSX

## 简介

1. 全称:  JavaScript XML
2. react定义的一种类似于XML的JS扩展语法: JS + XML本质是`React.createElement(component, props, …children)`方法的语法糖
3. 作用: 用来简化创建虚拟DOM 
	1) 写法：`var ele = <h1>Hello JSX!</h1>`
	2) 注意1：它不是字符串, 也不是HTML/XML标签
	3) 注意2：它最终产生的就是一个JS对象
4. 标签名任意: HTML标签或其它标签
5. 标签属性任意: HTML标签属性或其它
6. 基本语法规则
	1) 遇到 <开头的代码, 以标签的语法解析: html同名标签转换为html同名元素, 其它标签需要特别解析
	2) 遇到以 { 开头的代码，以JS语法解析: 标签中的js表达式必须用{ }包含
7. babel.js的作用
	1) 浏览器不能直接解析JSX代码, 需要babel转译为纯JS的代码才能运行
	2) 只要用了JSX，都要加上type=“text/babel”, 声明需要babel来处理

## 实例
```javascript
    const VDOM = (
      <div>
        <h2 className="title" id={myId.toLowerCase()}>
          <span style={{color:'white',fontSize:'29px'}}>{myData.toLowerCase()}</span>
        </h2>
        <h2 className="title" id={myId.toUpperCase()}>
          <span style={{color:'white',fontSize:'29px'}}>{myData.toLowerCase()}</span>
        </h2>
        <input type="text"/>
      </div>
    )
```

1. 定义虚拟DOM时，不要写引号。
2. 标签中混入JS表达式时要用`{}`。
3. 样式的类名指定不要用class，要用className。
4. 内联样式，要用style={{key:value}}的形式去写。
5. 只有一个根标签
6. 标签必须闭合
7. 标签首字母

* 若小写字母开头，则将该标签转为html中同名元素，若html中无该标签对应的同名元素，则报错。
* 若大写字母开头，react就去渲染对应的组件，若组件没有定义，则报错。
