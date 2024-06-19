# typeof

在 TS 中用于类型表达时，`typeof` 可以用于从一个变量上获取它的类型。

举一个没有卵用的例子：

```typescript
const value: number = 10;

const value2: typeof vlaue = 100;       // const value2: number
```

但是请注意下面这种情况：

```typescript
const value = 10;

const value2: typeof vlaue = 100;       

// const value2: 10
// ERROR: Type '100' is not assignable to type '10' 
```

经测试，`number` `string` `boolean` 类型在没有声明而直接赋值的情况下，都会存在这个问题

#### 举个有点用的例子

对于对象，数组，函数类型来讲，这个还是有点用的。[参考](https://link.juejin.cn/?target=https%3A%2F%2Fdev.to%2Fandreasbergqvist%2Ftypescript-get-types-from-data-using-typeof-4b9c "https://dev.to/andreasbergqvist/typescript-get-types-from-data-using-typeof-4b9c")

```typescript
const data = {
  value: 123,
  text: 'text',
  subData: {
    value: false
  }
};

type Data = typeof data;

// type Data = {
//   value: number;
//   text: string;
//   subData: {
//     value: boolean;
//   };
// }
```

  
