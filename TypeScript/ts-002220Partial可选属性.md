# 可选属性

## Partial可选属性

**Partial**可以将已声明的类型中的所有属性标识为**可选的**。

> ⚠️：只能做一层处理，多层处理使用**DeepPartial**

```typescript
// 该类型已内置在TypeScript中
type Partial<T> = {
    [P in keyof T]?: T[P]
};
```

示例：

```typescript
interface Rectangle {
    x: number;
    y: number;
    width: number;
    height: number;
}

type PartialRectangle = Partial<Rectangle>;
// 等价于
type PartialRectangle = {
    x?: number;
    y?: number;
    width?: number;
    height?: number;
}

let rect: PartialRectangle = {
    width: 100,
    height: 200
};
```

在上述示例中由于我们使用Partial将所有属性标识为可选的，因此最终rect对象中虽然只包含width和height属性，但是编译器依旧没有报错，当我们不能明确地确定对象中包含哪些属性时，我们就可以通过Partial来声明。

## DeepPartial

可做多层处理

```typescript
// 该类型已内置在TypeScript中
type DeepPartial<T> = {
     // 如果是 object，则递归类型
    [U in keyof T]?: T[U] extends object
      ? DeepPartial<T[U]>
      : T[U]
};

```

示例：

```typescript
type PartialedWindow = DeepPartial<Window>; 
// 现在window上所有属性都变成了可选啦
```


