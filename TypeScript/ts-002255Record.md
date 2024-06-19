# Record

**Record**可以获取已定义类型中所有可能值来设置 `key` 以及 `value` 的类型

```typescript
// 该类型已内置在TypeScript中
type Record<K extends keyof any, T> = {
    [P in K]: T;
};
```

示例：

```typescript
type CurRecord = Record<'a' | 'b' | 'c', UserInfo>; 
// { a: UserInfo; b: UserInfo; c: UserInfo; }
```

