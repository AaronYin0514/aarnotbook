# Readonly

**Readonly**可以将已声明的类型中的所有属性标识为**只读的**。

```typescript
// 该类型已内置在TypeScript中
type Readonly<T> = { 
    readonly [P in keyof T]: T[P] 
};
```