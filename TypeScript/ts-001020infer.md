# infer

关于 `infer` 操作符，这个可以用来进行类型推测。举个简单的小例子：

在 TS 中，如果我们在 `generator` 函数中使用了 `yield` 表达式，我们就会丢失类型。比如下面这样：

```typescript
function returnSomething() {
    // return something;
}

function* task() {
    // 这里的 result 在TS中是没有拿到正确的函数返回类型的，大家可以试一下
    const result = yield returnSomething();
}
```

那为了解决类似的问题，TS 为我们内置了 `ReturnType` 的映射类型：

```typescript
function* task() {
    // 这里的 result 在TS中是没有拿到正确的函数返回类型的，大家可以试一下
    const result: ReturnType<typeof returnSomething> = yield returnSomething();
}
```

那我们来看一下 `ReturnType` 是如何做的呢，其实很简单，就是用到了 `infer`:

```typescript
type ReturnType<T extends (...args: any[]) => any> = T extends (...args: any[]) => infer R ? R : any;
```

`infer P` 中的 `P` 即是表示待推断的返回值类型。
