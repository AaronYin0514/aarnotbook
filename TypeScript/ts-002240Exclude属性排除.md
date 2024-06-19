# Exclude属性排除

**Exclude**可以从一个已声明的类型中**排除**掉部分属性。

```typescript
// 该类型已内置在TypeScript中
// 这里使用了条件类型(Conditional Type)，和JS中的三目运算符效果一致
type Exclude<T, U> = T extends U ? never : T;
```

示例：

```typescript
interface User {
    id: number;
    name: string;
    age: number;
    gender: number;
    email: string;
}

type keys = keyof User; // -> "id" | "name" | "age" | "gender" | "email"

type ExcludeUser = Exclude<keys, "age" | "email">;
// 等价于
type ExcludeUser = "id" | "name" | "gender";
```

在上述示例中我们通过在ExcludeUser中传入我们不需要关心的age和email属性，Exclude会帮助我们将不需要的属性进行剔除，留下的属性id，name和gender即为我们需要关心的属性。一般来说，Exclude很少单独使用，可以与其他类型配合实现更复杂更有用的功能。