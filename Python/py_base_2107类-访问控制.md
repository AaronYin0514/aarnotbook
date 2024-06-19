---
tags: [python, class]
---
# 访问控制

## 类的属性和方法

### 私有属性和方法

在类定义体中的变量或方法，如果类的属性或方法名称以两个下划线开头并且不以两个下划线结束（__xx），则该属性或方法为私有的。即只能在类内部使用。在类的外部和子类中都不能够使用。

```python
class Person:

    def __init__(self, name, height, weight):
        self.name = name
        self._height = height
        self.__weight = weight
        
    def __str__(self):
        return '我的名字是' + self.name + '。' +  + '我的身高是' + self.__height + 'cm。'

    def __weigh(self):
        print("称体重: %d"%self.__weight)

    def _takeAShower(self):
        print("洗澡")
        self.__weigh()

zhangSan = Person("张三", 180, 200)
print(zhangSan.name) # 张三
print(zhangSan._height) # 180
print(zhangSan.__weight) # ❌ AttributeError: 'Person' object has no attribute '__weight'
zhangSan.__weigh() # ❌ AttributeError: 'Person' object has no attribute '__weigh'
```

### 伪私有

在传统的面向对象语言，比如Java，是完全面向对象编程的。这类语言，通常类里面的属性有三种类型，public、private、protected。

而Python 类的所有的属性类型，都是公开类型，或者说有一些伪私有。

```python
print(zhangSan._Person__weight) # 200
```

Python解释器会将带有'__'前缀的属性和方法，重命名为'_类名__属性名'。所以demo中可以访问到Person类的'私有属性'。


## 模块

> 模块参考[《模块》]()

[[2300模块]]

### 模块私有

如果在模块中的变量、函数或类名字以单下划线开头（_xx），则该变量、函数或类为模块私有，通过`import module_name from *`的方式不会将这些变量、函数或类导入；

rest.py文件：

```python
PI = 3.14
_MY_PI = 3.14

def sayHello():
    print('Hello,Python')
def _mySayHello():
    print('Hello,Python')

class Person(object):
    def __str__(self):
        return '我是Person对象'
class _Person(object):
    def __str__(self):
        return '我是_Person对象'
```

test.py文件

```python
from rest import *

print(PI) # 3.14
print(_MY_PI) # ❌ NameError: name '_MY_PI' is not defined

sayHello() # Hello,Python
_mySayHello() # ❌ NameError: name '_mySayHello' is not defined

p = Person()
print(p) # 我是Person对象
myp = _Person() # ❌ NameError: name '_Person' is not defined
```

## 访问

但仍然可以通过直接引入访问

```python
from rest import PI, _MY_PI

print(PI) # 3.14
print(_MY_PI) # 3.14
```