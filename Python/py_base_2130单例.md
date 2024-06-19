---
tags: [python, singleton]
---
# 单例

## __new__方法

```python
class A(object):
    def __new__(cls):
        print("这是 new 方法")
        return object.__new__(cls)
    
    def __init__(self):
        print("这是 init 方法")

A()
```

* `__new__`方法至少有一个参数cls，代表要实例化的类，此参数在实例化时由Python解释器自动提供。
* `__new__`必须有返回值，返回实例化出来的实例对象。一般返回父类`__new__`出来的对象。
* `__init__`至少有一个参数self，就是这个`__new__`返回的实例，`__init__`在`__new__`的基础上可以完成一些其它初始化的动作，`__init__`不需要返回值

## 单例

**单例**：确保某一个类只有一个实例，这个类称为单例类，单例模式是一种对象创建型模式。

### 创建单例——保证只有一个对象

```python
class Singleton(object):
    __instance = None

    def __new__(cls, age, name):
        [[如果类数字能够__instance没有或者没有赋值]]
        [[那么就创建一个对象，并且赋值为这个对象的引用，保证下次调用这个方法时]]
        [[能够知道之前已经创建过对象了，这样就保证了只有1个对象]]
        if not cls.__instance:
            cls.__instance = object.__new__(cls)
        return cls.__instance

a = Singleton(18, "dongGe")
b = Singleton(8, "dongGe")

print(id(a))
print(id(b))
```

### 创建单例——保证只执行一次`__init__`方法

```
class Singleton(object):
    __instance = None
    __first_init = False

    def __new__(cls, age, name):
        if not cls.__instance:
            cls.__instance = object.__new__(cls)
        return cls.__instance

    def __init__(self, age, name):
        if not self.__first_init:
            self.age = age
            self.name = name
            Singleton.__first_init = True


a = Singleton(18, "dongGe")
b = Singleton(8, "dongGe")

print(id(a))
print(id(b))


print(a.age)
print(b.age)

a.age = 19
print(b.age)
```








