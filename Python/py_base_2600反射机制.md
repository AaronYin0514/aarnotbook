# 反射机制

反射机制（Reflection）是编程语言的一种能力，允许程序在运行时检查和修改自身的结构和行为。在 Python 中，反射机制可以用于动态地获取对象的信息（如类型、属性和方法），以及动态地调用方法或修改属性。Python 提供了一系列内置函数来支持反射机制，包括 `getattr`、`setattr`、`hasattr`、`delattr` 以及内置模块 `inspect`。

### 反射机制的基本操作

1. **获取对象的类型**：可以使用 `type()` 和 `isinstance()` 函数。

```python
x = 10
print(type(x))  # 输出: <class 'int'>
print(isinstance(x, int))  # 输出: True
```

2. **获取对象的属性和方法**：可以使用 `dir()` 函数。

```python
class MyClass:
    def __init__(self, name):
        self.name = name
    
    def greet(self):
        print(f"Hello, {self.name}!")

obj = MyClass("Alice")
print(dir(obj))  # 输出对象的所有属性和方法列表
```

3. **检查对象是否有特定属性或方法**：可以使用 `hasattr()` 函数。

```python
print(hasattr(obj, 'name'))  # 输出: True
print(hasattr(obj, 'greet'))  # 输出: True
print(hasattr(obj, 'age'))  # 输出: False
```

4. **获取对象的属性值**：可以使用 `getattr()` 函数。

```python
name = getattr(obj, 'name')
print(name)  # 输出: Alice

# 获取方法并调用
greet_method = getattr(obj, 'greet')
greet_method()  # 输出: Hello, Alice!
```

5. **设置对象的属性值**：可以使用 `setattr()` 函数。

```python
setattr(obj, 'age', 30)
print(obj.age)  # 输出: 30
```

### `inspect` 模块

Python 的 `inspect` 模块提供了更多反射功能，可以用于检查活跃的对象、模块、类、方法等。

```python
import inspect

class MyClass:
    def __init__(self, name):
        self.name = name
    
    def greet(self):
        print(f"Hello, {self.name}!")

obj = MyClass("Alice")

# 获取类的信息
print(inspect.getmembers(obj))

# 检查是否是方法
print(inspect.ismethod(obj.greet))  # 输出: True

# 获取方法的源代码
print(inspect.getsource(MyClass.greet))
```




