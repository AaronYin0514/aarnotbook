---
tags: [python, class]
---
# 属性

```python
class Person:

  def __init__(self, name):
    self.name = name
    self.age = 1
  
  def speak(self, words):
    print('My name is ' + self.name + '. ' + words)

  def __str__(self):
    return '我的名字是' + self.name + '。' + '我的年龄是' + str(self.age) + '岁。'

person = Person('Guido')
person.age = -18

print(person) # 我的名字是Guido。我的年龄是-18岁。
```

现在添加了一个age属性给Person。但是打印结果有些奇怪，年龄不可能是负数，但是我们可以赋值给它任何值。

## setter方法 & getter方法

```python
class Person:

  def __init__(self, name):
    self.name = name
    self.age = 1

  def setAge(self, age):
    if age <= 0:
      self.age = 1
    else:
      self.age = age
  
  def getAge(self):
    return self.age

  def __str__(self):
    return '我的名字是' + self.name + '。' + '我的年龄是' + str(self.age) + '岁。'

person = Person('Guido')
person.setAge(-18)
print('年龄:' + str(person.getAge()) + '岁。') # 年龄:1岁。
```

我们可以通过编写setter和getter方法的方式，间接设置和访问实例属性，这样方便我们在设置和获取时对数据进行校验。set get方法格式通常如下：

```python
  def setAge(self, age):
    if age <= 0:
      self.age = 1
    else:
      self.age = age
  
  def getAge(self):
    return self.age
```

## @property

setter和getter方法在所有面向对象编程语言都非常重要。Python提供了**@property装饰器**，使我们可以以属性的方式调用setter和getter方法，更急便捷。

```python
class Person:

    def __init__(self, name, age=1):
        self.name = name
        self.__age = age

    @property
    def age(self):
        print('调用getter方法')
        return self.__age

    @age.setter
    def age(self, age):
        print('调用setter方法')
        if age <= 0:
            self.__age = 1
        else:
            self.__age = age
        
    def __str__(self):
        return '我的名字是' + self.name + '。' + '我的年龄是' + str(self.age) + '岁。'

person = Person('Guido')
person.age = -18
print('年龄:' + str(person.age) + '岁。') # 年龄:1岁。
```

> `__age`是私有属性，详见《类-访问控制》
> 装饰器详见《装饰器》

语法：

```python
    @property # getter方法前加@property装饰器。getter方法名不需要加get
    def age(self):
        print('调用getter方法')
        return self.__age

    @age.setter # getter方法前加@age.setter。setter方法名不需要加set
    def age(self, age):
        print('调用setter方法')
        if age <= 0:
            self.__age = 1
        else:
            self.__age = age
```

使用：使用方式与属性相同。

```python
person = Person('Guido')
person.age = -18
print(person.age)
```

## `__slots__`

```python
class Person(object):
    pass

zhangsan = Person()
zhangsan.name = '张三'
zhangsan.age = 18
zhangsan.xxxxxxx = 'yyyyyyy'

print(zhangsan.xxxxxxx) # yyyyyyy
```

可以看出，Python定义属性比较随意，我们可以任意动态添加属性。‘xxxxxxx’属性显然与业务无关，我们怎样限制用户不能随意设置呢？

```python
class Person(object):
    __slots__ = ('name', 'age')

zhangsan = Person()
zhangsan.name = '张三'
zhangsan.age = 18
zhangsan.xxxxxxx = 'yyyyyyy' # ❌ AttributeError: 'Person' object has no attribute 'xxxxxxx'
```

`__slots__`可限制类可添加的属性，它的值是一个元组。`__slots__`定义的属性仅对当前类实例起作用，对继承的子类是不起作用的。

```python
class Student(Person):
    pass

lisi = Student()
lisi.score = 100
print(lisi.score) # 100
```

## 属性拦截器

在调用实例对象属性时，会先调用属性拦截器`__getattribute__`，它的返回值就是调用属性接收的值。

```python
class Person(object):
    __slots__ = ('name', 'book')

    def __init__(self, name):
        super().__init__()
        self.name = name
    
    def write(self, book):
        self.book = book

    def __getattribute__(self, obj):
        if obj == 'book':
            book = object.__getattribute__(self, obj)
            return  '%s - 版权归%s'%(book, self.name)
        else:
            return object.__getattribute__(self, obj)

zhangsan = Person('张三')
zhangsan.write('Life is short, you need Python!')

print(zhangsan.book) # Life is short, you need Python! - 版权归张三
```

demo中张三写了一本书。在访问书的属性时，都加上版权归属。所以属性拦截器可以帮助我们在返回给用户前做一些调整。

