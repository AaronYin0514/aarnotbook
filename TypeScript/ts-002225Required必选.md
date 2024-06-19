# Required

**Required**可以将已声明的类型中的所有属性标识为**必选的**。

```typescript
// 该类型已内置在TypeScript中
type Required<T> = { 
    [P in keyof T]-?: T[P] 
};
```

其中 `-?` 是代表移除 `?` 这个 modifier 的标识。再拓展一下，除了可以应用于 `?` 这个 modifiers ，还有应用在 `readonly` ，比如 `Readonly<T>` 这个类型

