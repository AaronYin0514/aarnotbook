# keyof索引查询

在TypeScript中的`keyof`有点类似于JS中的`Object.keys()`方法，但是区别在于前者遍历的是类型中的字符串索引，后者遍历的是对象中的键名

示例：

```typescript
interface Rectangle {
    x: number;
    y: number;
    width: number;
    height: number;
}

type keys = keyof Rectangle;
// 等价于
type keys = "x" | "y" | "width" | "height";
```

```typescript
// 这里使用了泛型，强制要求第二个参数的参数名必须包含在第一个参数的所有字符串索引中
function getRectProperty<T extends object, K extends keyof T>(rect: T, property: K): T[K] {
    return rect[property];
} 

let rect: Rectangle = {
    x: 50,
    y: 50,
    width: 100,
    height: 200
};

console.log(getRectProperty(rect, 'width')); // -> 100
console.log(getRectProperty(rect, 'notExist'));
// -> Argument of type '"notExist"' is not assignable to parameter of type '"width" | "x" | "y" | "height"'.
```

在上述示例中我们通过使用keyof来限制函数的参数名property必须被包含在类型Rectangle的所有字符串索引中，如果没有被包含则编译器会报错，可以用来在编译时检测对象的属性名是否书写有误。
