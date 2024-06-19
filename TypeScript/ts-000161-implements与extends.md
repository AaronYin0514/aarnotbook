---
tags: ts typescript
---

# implements与extends

**implements**

实现，一个新的类，从父类或者接口实现所有的属性和方法，同时可以重写属性和方法，包含一些新的功能

**extends**

继承，一个新的接口或者类，从父类或者接口继承所有的属性和方法，不可以重写属性，但可以重写方法

```ts
interface IPerson {
  age: number;
  name: string;
}

interface IPeoPle extends IPerson {
  sex: string;
}

class User implements IPerson {
  age: number;
  name: string;
}
interface IRoles extends User{

}
class Roles extends User{
  
}
```

## 注意

-   接口不能实现接口或者类，所以实现只能用于类身上,即类可以实现接口或类
-   接口可以继承接口或类
-   类不可以继承接口，类只能继承类
-   可多继承或者多实现

