#  extends

`extends` 在类型表达时，有下面两种用法：

## 1.  用于类型的继承

```typescript
interface Person {
    name: string;
    age: number;
}

interface Player extends Person {
    item: 'ball' | 'swing';
}
```

## 2.  判断是否是能赋值给另一个类型

```typescript
// 如果 T 可以满足类型 Person 则返回 Person 类型，否则为 T 类型
type IsPerson<T> = T extends Person ? Person : T;
```

## 实例

#### 数组第一个元素

实现一个通用`First<T>`，它接受一个数组`T`并返回它的第一个元素的类型。

```ts
//answer1
type First<T extends any[]> = T extends [] ? never : T[0]

//answer2
type First<T extends any[]> = T['length'] extends 0 ? never : T[0]

//answer3
type First<T extends any[]> = T extends [infer A, ...infer rest] ? A : never
```





