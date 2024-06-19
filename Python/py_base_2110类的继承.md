---
tags: [python, class]
---
# 类的继承

## 继承

```python
import random
from datetime import datetime

# 父类
class Person:

  def __init__(self, name, birthday_str):
    self.__id = self.__create_id() # 身份证号
    self.name = name # 名字
    self.__birthday = datetime.strptime(birthday_str, '%Y-%m-%d') # 生日

  def get_birthday(self):
    return self.__birthday
  
  def get_id(self):
    return self.__id

  def is_birthday(self):
    birthday_str = self.__birthday.strftime('%m-%d')
    now_str = datetime.now().strftime('%m-%d')
    if birthday_str == now_str:
      print(self.name + ", 祝你生日快乐 🎂")
    else:
      print('今天不是你的生日')
  
  def __create_id(self):
    return '%f%d'%(datetime.now().timestamp(), random.randint(0, 1000))

# 子类Teacher继承于Person，将Person写在括号里
class Teacher(Person):
  pass
```

子类可以继承父类的**公开属性**和**公开方法**。测试：

```python
zhang_san = Person('张三', '1980-05-04')
print(zhang_san.name) # 张三
zhang_san.is_birthday() # 张三, 祝你生日快乐 🎂

li_si = Teacher('李四', '1985-05-04')
print(li_si.name) # 李四
li_si.is_birthday() # 李四, 祝你生日快乐 🎂
```

子类**不能**直接调用父类的**私有属性**和**私有方法**：

```python
# 子类
class Student(Person):
  def test(self):
    print(self.__birthday) # ❌ 程序错误 AttributeError: 'Student' object has no attribute '_Student__birthday'
    print(self.__create_id(_)) # ❌ 程序错误 AttributeError: 'Student' object has no attribute '_Student__create_id'

wang_wu = Student('王五', '2014-05-04')
wang_wu.test()
```

## 扩展属性和方法

```python
import random
from datetime import datetime

class Person:

  def __init__(self, name):
    self.name = name # 名字

class Teacher(Person):

  def work(self):
    print('我叫%s，我是老师，我正在教书...'%(self.name, self.book))

# 测试
li_si = Teacher('李四')
li_si.book = "《Python从入门到放弃》"
li_si.work() # 我叫李四，我是老师，我正在教《Python从入门到放弃》
```

类可以通过继承继承父类的所有公开方法和属性，也能扩展属于子类特有的方法和属性。demo中为Teacher类扩展了book属性和work方法。

## 重写父类方法

```python
class Person:

  def __init__(self, name):
    self.name = name # 名字

  def work_out(self):
    return '人人都要锻炼好身体！'

class Teacher(Person):

  def __init__(self, name, book):
    Person.__init__(self, name)
    self.book = book
  
  def work_out(self):
    # str = super(Teacher, self).work_out()
    str = super().work_out()
    return str + '我喜欢跑步!'

  def work(self):
    print('我叫%s，我是老师，我正在教%s'%(self.name, self.book))
```

子类可以继承父类的方法和属性，是因为，程序在调用某个实例对象的方法或属性时，如果没有找到，就会去它的父类中再找，如果找到了就执行该方法，如果没有找到就再到父类的父类中找...

所以子类重写了父类的方法，那么程序在子类中就找到并直接执行。如demo中的`work_out`方法。

**子类调用父类方法**

如demo的构造方法，父类的构造方法已经将属性初始化好了。而子类在重写父类方法时还要笨笨的重新写一遍？我们需要在子类中调用父类方法。

方法一：通过父类名称调用，注意self参数必须要传

```python
Person.__init__(self, name)
```

方法二：通过super(Teacher, self)调用，注意不需要传self

```python
super(Teacher, self).work_out()
```

方法三：super()方法与方法二方法等价

```python
super().work_out()
```

## 多继承

```python
class Person(object):

  def work_out(self):
    print('人人都要锻炼好身体！')

class Work(object):

  def __init__(self):
    self.money = 0

  def work(self):
    self.money += 100
    print('人人都要工作... 我的存款有%d元'%self.money)

class Teacher(Person, Work):

  def life(self):
    Person.work_out(self)
    Work.work(self)
    super().work_out()
    super().work()

wang_wu = Teacher()
wang_wu.work_out()
wang_wu.work()
wang_wu.life()
```

定义多继承只需要在括号中同时写上多个父类的类名。单继承方法查找的顺序是子类->父类->父类的父类...，那么多继承的方法查找顺序是怎样的呢？

```python
print(Teacher.__mro__) # (<class '__main__.Teacher'>, <class '__main__.Person'>, <class '__main__.Work'>, <class 'object'>)
```

通过调用`__mro__`可以看到多继承方法查找的顺序。编程中应尽量避免父类之间方法重名问题。