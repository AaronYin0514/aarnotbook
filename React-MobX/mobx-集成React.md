---
tags: react, mobx
---

# 集成React（react-integration）

用法:

```javascript
import { observer } from "mobx-react-lite" // Or "mobx-react".

const MyComponent = observer(props => ReactElement)
```

MobX 可以独立于 React 运行, 但是他们通常是结合在一起使用, 在 [Mobx的宗旨（The gist of MobX）](https://zh.mobx.js.org/the-gist-of-mobx.html) 一文中你会经常看见集成React最重要的一部分：用于包裹React Component的 `observer` [HOC](https://reactjs.org/docs/higher-order-components.html)方法。

`observer` 是你可以自主选择的，[在安装时（during installation）](https://zh.mobx.js.org/installation.html#installation)独立提供的 React bindings 包。 在下面的例子中,我们将使用更加轻量的[`mobx-react-lite` 包](https://github.com/mobxjs/mobx/tree/main/packages/mobx-react-lite)。

```javascript
import React from "react"
import ReactDOM from "react-dom"
import { makeAutoObservable } from "mobx"
import { observer } from "mobx-react-lite"

class Timer {
    secondsPassed = 0

    constructor() {
        makeAutoObservable(this)
    }

    increaseTimer() {
        this.secondsPassed += 1
    }
}

const myTimer = new Timer()

//被`observer`包裹的函数式组件会被监听在它每一次调用前发生的任何变化
const TimerView = observer(({ timer }) => <span>Seconds passed: {timer.secondsPassed}</span>)

ReactDOM.render(<TimerView timer={myTimer} />, document.body)

setInterval(() => {
    myTimer.increaseTimer()
}, 1000)
```

**提示:** 你可以在 [在线编译器CodeSandbox](https://codesandbox.io/s/minimal-observer-p9ti4?file=/src/index.tsx)中尝试上面的例子。

`observer` HOC 将自动订阅 React components 中任何 _在渲染期间_ 被使用的  _可被观察的对象_ 。 因此, 当任何可被观察的对象 _变化_ 发生时候 组件会自动进行重新渲染（re-render）。 它还会确保组件在 _没有变化_ 发生的时候不会进行重新渲染（re-render）。 但是, 更改组件的可观察对象的不可读属性, 也不会触发重新渲染（re-render）。

在实际项目中，这一特性使得MobX应用程序能够很好的进行开箱即用的优化，并且通常不需要任何额外的代码来防止过度渲染。

要想让`observer`生效, 并不需要关心这些对象 _如何传递到_ 组件的（它们只要能传递给组件即可 ·译者注）, 只需要关心他们是否是可读的。 深层嵌套的可观察对象也没有问题, 复杂的表达式类似 `todos[0].author.displayName` 也是可以使用的。 与其他必须显式声明或预先计算数据依赖关系的框架（例如 selectors）相比，这种发生的订阅机制就显得更加精确和高效。

## 本地与外部状态（Local and external state）

在 Mobx 可以非常灵活的组织或管理（state）, 从（技术角度讲）它不关心我们如何读取可观察对象，也不关心他们来自哪里。 下面的例子将通过不同的设计模式去使用被 `observer`包裹的组件。

### `observer` 组件中使用外部状态 （Using external state in `observer` components）

使用 props

使用全局变量

使用 React context

可被观察对象可以通过组件的props属性传入 (在下面的例子中):

```javascript
import { observer } from "mobx-react-lite"const myTimer = new Timer() // See the Timer definition above.const TimerView = observer(({ timer }) => <span>Seconds passed: {timer.secondsPassed}</span>)// 通过props传递myTimer.ReactDOM.render(<TimerView timer={myTimer} />, document.body)
```

### 在`observer` 组件中使用全局可观察对象（Using local observable state in `observer`components）

因为使用 `observer` 的可观察对象可以来自任何地方, 他们也可以使用local state（全局的state·译者注）。 再次声明，不同操作方式对于我们而言都是有价值的。

**useState 和 observable class**

使用全局可观察对象的最简单的方式就是通过`useState`去存储一个全局可观察对象的引用。 需要注意的是, 因为我们不需要替换全局可观察对象的引用,所以我们其实可以完全不声明`useState`的更新方法:

```javascript
import { observer } from "mobx-react-lite"import { useState } from "react"const TimerView = observer(() => {    const [timer] = useState(() => new Timer()) // Timer的定义在上面（正如上面所说的那样这里我们忽略了更新方法的定义·译者注）。    return <span>Seconds passed: {timer.secondsPassed}</span>})ReactDOM.render(<TimerView />, document.body)
```

如果你想要类似我们官方的例子那样自动更新 timer , 使用`useEffect` 可能是 React 中比较典型的写法：

```javascript
useEffect(() => {    const handle = setInterval(() => {        timer.increaseTimer()    }, 1000)    return () => {        clearInterval(handle)    }}, [timer])
```

**useState 与全局可观察对象**

如刚才说的那样, 直接创建一个可观察的对象，而不是使用classes（这里的类指的是全局定义的Mobx state，在它们是使用class声明的）。 我们可以参考 [observable](https://zh.mobx.js.org/observable-state.html#observable)这篇文章：

```javascript
import { observer } from "mobx-react-lite"import { observable } from "mobx"import { useState } from "react"const TimerView = observer(() => {    const [timer] = useState(() =>        observable({            secondsPassed: 0,            increaseTimer() {                this.secondsPassed++            }        })    )    return <span>Seconds passed: {timer.secondsPassed}</span>})ReactDOM.render(<TimerView />, document.body)
```

**useLocalObservable hook**

`const [store] = useState(() => observable({ /* something */}))` 是非常通用的一套写法， 为了简化这个写法我们可以调用`mobx-react-lite` 包中的 [`useLocalObservable`](https://github.com/mobxjs/mobx-react#uselocalobservable-hook) hook ,可以将上面的例子简化成：

```javascript
import { observer, useLocalObservable } from "mobx-react-lite"import { useState } from "react"const TimerView = observer(() => {    const timer = useLocalObservable(() => ({        secondsPassed: 0,        increaseTimer() {            this.secondsPassed++        }    }))    return <span>Seconds passed: {timer.secondsPassed}</span>})ReactDOM.render(<TimerView />, document.body)
```

### 你可能并不需要全局的可观察状态 （You might not need locally observable state）

通常来讲，我们推荐在编写全局公用组件的时候不要立刻使用Mobx的可观察能力， 因为从技术角度来讲他可能会使你无法使用一些React 的 Suspense 的方法特性。 总的来说，使用Mobx的可观察能力会捕获组件间的域状态（domain data）可能会 (包含子组件的)。像是todo item, users, bookings, 等等（就是说最好不要用mobx共享一些组件内的状态·译者注）。

状态类似获取UI state, 类似加载的 state, 选择的 state,等等, 最好还是使用 [`useState` hook](https://reactjs.org/docs/hooks-state.html), 这样可以让你使用高级的 React suspense特性。

使用Mobx的可观察能力作为 React components 的一种状态补充，比如出现以下情况： 1) 层级很深, 2) 拥有计算属性 3) 需要共享状态给其它 `observer` components。

## 始终在`observer` 组件中使用可观察能力（Always read observables inside `observer` components）

你可能会感到疑惑, 我应该什么时候使用 `observer`? 大体上说: _ `observer`应用于所有组件的可观察数据 _ 。

`observer` 是使用修饰模式增强你的组件, 而不是它调用你的组件. 所以通常所有的组件都可能用了 `observer`，但是不要担心， 它不会导致性能损失。从另一个角度讲, 更多的 `observer` 组件可以使渲染更高效，因为它们更新数据的颗粒度更细。

### [](https://zh.mobx.js.org/react-integration.html#%E5%B0%8F%E8%B4%B4%E5%A3%AB-%E5%B0%BD%E5%8F%AF%E8%83%BD%E6%99%9A%E5%9C%B0%E4%BB%8E%E5%AF%B9%E8%B1%A1%E4%B8%AD%E8%8E%B7%E5%8F%96%E5%80%BC)小贴士: 尽可能晚地从对象中获取值

只要你传递引用，`observer` 就可以很好的工作。只要获取到内部的属性，基于 `observer` 的组件 就会渲染到 DOM / low-level components（DOM一般是浏览器环境，low-level components 一般是RN环境·译者注）。换句话说， `observer` 会根据实际情况响应你定义的对象中的值的'引用'。

下面的例子中, `TimerView` 组件**不会**响应未来的更新，因为`.secondsPassed`不是在 `observer`组件内部读取的而是在外部读取的,因此它_不会_被追踪到：

```javascript
const TimerView = observer(({ secondsPassed }) => <span>Seconds passed: {secondsPassed}</span>)

React.render(<TimerView secondsPassed={myTimer.secondsPassed} />, document.body)
```

需要注意的一点是它不同于其它的观念模式库像是 `react-redux`那样, redux 中强调尽可能早的获取和传递原始值以获得更好的副作用响应。 如果你还是没有理解, 可以先阅读 [理解响应式（Understanding reactivity）](https://zh.mobx.js.org/understanding-reactivity.html) 这篇文章。

### [](https://zh.mobx.js.org/react-integration.html#%E4%B8%8D%E8%A6%81%E5%B0%86%E5%8F%AF%E8%A7%82%E5%AF%9F%E5%AF%B9%E8%B1%A1%E4%BC%A0%E9%80%92%E5%88%B0-%E4%B8%8D%E6%98%AFobserver%E7%9A%84%E7%BB%84%E4%BB%B6%E4%B8%AD%EF%BC%88dont-pass-observables-into-components-that-arent-observer%EF%BC%89)不要将可观察对象传递到 不是`observer`的组件中（Don't pass observables into components that aren't `observer`）

通过`observer`包裹的组件 _只可以_ 订阅到在 _他们自己_ 渲染的期间的可观察对象. 如果要将可观察对象 objects / arrays / maps 传递到子组件中, 他们必须被 `observer` 包裹。 通过callback回调的组件也是一样。

如果你非要传递可观察对象到未被`observer`包裹的组件中， 要么是因为它是第三方组件，要么你需要组件对Mobx无感知，那你必须在传递前 [转换可观察对象为显式 （convert the observables to plain JavaScript values or structures）](https://zh.mobx.js.org/observable-state.html#%E5%B0%86-observable-%E8%BD%AC%E6%8D%A2%E5%9B%9E%E6%99%AE%E9%80%9A%E7%9A%84-javascript-%E9%9B%86%E5%90%88) 。

关于上述的详细描述, 可以看一下下面的使用 `todo` 对象的例子， 一个 `TodoView` (observer)组件和一个虚构的接收一组对象映射入参的不是`observer`的`GridRow`组件：

```javascript
class Todo {
    title = "test"
    done = true

    constructor() {
        makeAutoObservable(this)
    }
}

const TodoView = observer(({ todo }: { todo: Todo }) =>
   // 错误: GridRow 不能获取到 todo.title/ todo.done 的变更
   //       因为他不是一个观察者（observer。
   return <GridRow data={todo} />

   // 正确:在 `TodoView` 中显式的声明相关的`todo` ，
   //      到data中。
   return <GridRow data={{
       title: todo.title,
       done: todo.done
   }} />

   // 正确: 使用 `toJS`也是可以的, 并且是更清晰直白的方式。
   return <GridRow data={toJS(todo)} />
)
```

### [](https://zh.mobx.js.org/react-integration.html#%E5%9B%9E%E8%B0%83%E7%BB%84%E4%BB%B6%E5%8F%AF%E8%83%BD%E4%BC%9A%E9%9C%80%E8%A6%81observer%EF%BC%88-callback-components-might-require-observer%EF%BC%89)回调组件可能会需要`<Observer>`（ Callback components might require `<Observer>`）

想象一下在同样的例子中,  `GridRow` 携带一个 `onRender`回调函数。 `onRender` 是 `GridRow`渲染生命周期的一部分, 而不是 `TodoView` 的render (甚至在语法层面都能看出来)，我们不得不保证回调组件是一个 `observer` 组件。 或者，我们可以使用 [`<Observer />`](https://github.com/mobxjs/mobx-react#observer)创建一个匿名观察者：

```javascript
const TodoView = observer(({ todo }: { todo: Todo }) => {
    // 错误: GridRow.onRender 不能获得 todo.title / todo.done 中的改变
    //        因为它不是一个观察者（observer） 。
    return <GridRow onRender={() => <td>{todo.title}</td>} />

    // 正确: 将回调组件通过Observer包裹将会正确的获得变化。
    return <GridRow onRender={() => <Observer>{() => <td>{todo.title}</td>}</Observer>} />
})
```