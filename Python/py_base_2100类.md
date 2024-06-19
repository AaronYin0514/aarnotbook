---
tags: [python, class]
---
# 类

## 定义一个类

定义一个类的格式：

```
class 类名:
  方法列表
```

例如：定义一个Person类

```python
class Person:
  pass
```

> pass 在python中，如果还没有实现一个类或者函数等，可使用pass，防止运行错误

## 创建对象

创建对象格式:

```
对象名 = 类名()
```

例如创建一个person对象

```python
person = Person()
```

## 实例方法

### 定义

为Person类添加一个speak方法：

```python
class Person:
  
  def speak(self, words):
    print(words)
```

与普通函数不同，类实例方法至少有一个参数`self`，且必须是第一个参数，它指向调用方法的实例对象本身。除此以外的使用方法与函数完全相同。

### 调用

```python
person = Person()
person.speak('Life is short, you need Python.')
```

实例在调用实例方法时，不需要将self参数传入，因为python解释器已经帮你做了。其他参数正常传入即可。

## 属性

为实例对象添加一个属性，很简单，直接赋值即可。

```python
class Person:
  
  def speak(self, words):
    print('My name is ' + self.name + '. ' + words)

person = Person()
person.name = 'Guido'
person.speak('Life is short, you need Python.')
```

`person.name = 'Guido'`这样就为person添加了一个`name`属性。

## 构造方法

现在想给一个person对象出生时就起一个名字。怎样做到呢？事实上，`Person()`这段代码也调用了一个方法`__init__`，`__init__`方法叫做**构造方法**，它的作用是初始化对象的属性。

```python
class Person:

  def __init__(self, name):
    self.name = name
  
  def speak(self, words):
    print('My name is ' + self.name + '. ' + words)

person = Person('Guido')
person.speak('Life is short, you need Python.')
```

## `__str__` 描述一个对象

> 在python中，有很多方法的名称格式是__xxxx__，这些方法都有一些特殊作用。我们称之为**魔法方法**。

```python
print(person) # <__main__.Person object at 0x100c5dfd0>
```

尝试直接打印一个对像，结果是对象的内存地址。而我想知道对象的更加详细的信息，怎么办呢？

```python
class Person:

  def __init__(self, name):
    self.name = name
  
  def speak(self, words):
    print('My name is ' + self.name + '. ' + words)

  def __str__(self):
    return '我的名字是' + self.name + '.'

person = Person('Guido')
print(person) # 的名字是Guido.
```

这次打印的结果是“的名字是Guido.”。所以`__str__`方法返回一个字符串，`print`打印一个对象时，调用了该方法，将得到的字符串在控制台打印出来。

## `__del__` 析构方法

与`__init__`方法相对，在对象被系统释放时，会调用`__del__`方法，称之为**析构方法**。

```python
class FileManager:

  def __init__(self, path):
    self.__file = open(path, 'a')
    self.__file.write('这是我的style ~\n\n')

  def write(self, content):
    self.__file.write(content.center(7) + '\n')
  
  def __del__(self):
    print('我被释放了')
    self.__file.close()


manager = FileManager('test.txt')
manager.write('桃花庵歌')
manager.write('别人笑我太疯癫')
manager.write('我笑他人看不穿')
```

| 方法名 | 中文名 | 调用时机 | 作用 |
| ---- | ---- | ---- | ---- |
| __init__ | 构造方法 | 创建对象时 | 初始化属性 和 一些业务上需要的准备工作，例如打开一个文件 |
| __del__ | 析构方法 | 对象被释放时 | 做一些收尾的工作，例如关闭文件 |

## 引用计数

析构方法的作用是对象释放时被调用，那么对象是什么时候被释放的呢？每个对象都有一个引用计数，表示当前正在引用该对象的变量的个数。

如果引用计数大于0，说明正有变量在引用这它，说明它当前是有用的，正在工作，所以不可以释放。

一旦引用计数变成0，说明没有变量引用它，它也就没有了作用，那么系统就会将这个变量释放掉。

```python
import sys

class Person:
  pass

p1 = Person()
print("当前引用计数: ", sys.getrefcount(p1)) # 2

p2 = p1
print("当前引用计数: ", sys.getrefcount(p1)) # 3

del p2
print("当前引用计数: ", sys.getrefcount(p1)) # 2
```

可以通过`sys.getrefcount`查看当前对象的引用计数。需要注意的是`sys.getrefcount`也会使对象引用计数+1

## `==`判断

判断两个对象是否相等，一般是用 `==` 运算符， 如果是自定义对象，`==` 运算符可以被`__eq__`方法魔改

```python
class Person:
	def __init__(self, name):
		self.name = name

	def __eq__(self, __value: object) -> bool:
		return self.name == __value.name

a = Person('xiaoming')
b = Person('xiaohong')
c = Person('xiaoming')

print(a == b) # False
print(a == c) # True
```






