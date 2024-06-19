# never类型

`never`表示**永不存在的值的类型**。

1. `never`类型可以是任何类型的**子类型**
2. `never`类型可以赋值给任何类型
3. 没有类型可以作为`never`类型的子类型

示例：

```typescript
// 函数抛出异常
function throwError(message: string): never {
    throw new Error(message);
}

// 函数自动推断出返回值为never类型
function reportError(message: string) {
    return throwError(message);
}

// 无限循环
function loop(): never {
    while(true) {
        console.log(1);
    }
}

// never类型可以是任何类型的子类型
let n: never;
let a: string = n;
let b: number = n;
let c: boolean = n;
let d: null = n;
let e: undefined = n;
let f: any = n;

// 任何类型都不能赋值给never类型
let a: string = '123';
let b: number = 0;
let c: boolean = true;
let d: null = null;
let e: undefined = undefined;
let f: any = [];

let n: never = a;
// -> Type 'string' is not assignable to type 'never'.

let n: never = b;
// -> Type 'number' is not assignable to type 'never'.

let n: never = c;
// -> Type 'true' is not assignable to type 'never'.

let n: never = d;
// -> Type 'null' is not assignable to type 'never'.

let n: never = e;
// -> Type 'undefined' is not assignable to type 'never'.

let n: never = f;
// -> Type 'any' is not assignable to type 'never'.
```

## 实例1

```typescript
// ❌ 报错形式
interface IType { 
	age: number; 
	[key: string]: string; 
}
```


```typescript
// ✅ 正确方式
interface ITypeAge {
  age: number;
}  

interface ITypeKeyAny {
  age: never;
  [key: string]: string;
}

type Itype = ITypeAge | ITypeKeyAny;

```
