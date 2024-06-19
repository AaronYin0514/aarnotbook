---
tags: [python, module]
---
# 模块

## 引用模块

### import ...

通过`import 模块名1,模块名2...`来引用模块

```python
import math

print(math.sqrt(2)) # 调用时要加math模块名
```

### from ... import ...

如果只想引用指定函数，通过`from 模块名 import 函数名1,函数名2...`引用

```python
from math import sqrt

print(sqrt(2)) # 调用是不要加math模块名
```

如果想导入模块全部内容`from 模块名 import *`，此方法应该尽量少的使用。

```python
from math import *

print(sqrt(2))
```

### as 别名

如果模块间出现函数名冲突的情况，可以使用`as`给函数起一个别名

```python
from math import sqrt as my_sqrt

print(my_sqrt(2))
```

## 自定义模块

每一个py文件就是一个模块。通过import可以引用模块，import首先在当前目录查找，没有查找到再去系统目录中寻找。

所以模块名不要与系统模块名称冲突。

## 测试模块

### `__name__`

直接执行python脚本，脚本文件中`__name__`的值是'__main__'。如果python脚本是被其它文件当作模块引用，则打印出来的结果是模块名。

a.py文件

```python

print(__name__)

def add(a, b):
  return a + b

def main():
  print('测试代码...')

if __name__ == '__main__':
  main()
```

如果执行a.py文件，`__name__`值是'__main__'

b.py文件

```python
import a

print(a.add(10, 20))
```

执行b.py，则a.py中的`__name__`值是'a'

### 测试

通过import引用某模块时，会执行一遍模块中的代码。所以如果不加限制的调用测试代码，测试代码一旦被引用就会执行。

所以可以根据`__name__`的特性，下面`main`函数只有直接执行它自己才能被调用。

```python
def main():
  print('测试代码...')

if __name__ == '__main__':
  main()
```

## import搜索路径

刚刚说过模块搜索先从当前目录搜索，再从系统目录搜索。准确的说是按照`sys.path`的顺序遍历。

```python
import sys

print(sys.path)
```

## 循环导入

在a模块中导入了b模块，在b模块中又导入了a模块，将导致循环引用。