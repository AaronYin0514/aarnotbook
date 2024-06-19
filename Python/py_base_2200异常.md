e---
tags: [python, exception]
---
# 异常

## try...except... 捕获异常

```python
open('xxx.py', 'r') # ❌ FileNotFoundError: [Errno 2] No such file or directory: 'xxx.py'
```

读取一个不存在的文件将导致程序异常（FileNotFoundError），如果不对异常进行处理，将导致程序崩溃。正确的处理方式是捕获异常，提示用户或做出相应的处理，不能让程序崩溃。

### 捕获异常

```python
try:
  open('xxx.py', 'r')
except FileNotFoundError:
  print('文件不存在')

print('--- 程序执行到了这里 ---')
```

结果:

```
文件不存在
--- 程序执行到了这里 ---
```

如果`try`代码块中的程序没有发生异常，正常向下执行。反之程序会被打断，异常会被捕获，因为发生了FileNotFoundError类型异常，所以异常被下面代码块捕获。

```python
except FileNotFoundError:
  print('文件不存在')
```

### 捕获多个异常

现在虽然FileNotFoundError类型的异常可以被捕获，但是如果还有其它类型异常呢，比如TypeError，那么我们需要同时捕获这两个异常。

**方式一**

```python
try:
  str = "str" + 0
  open('xxx.py', 'r')
except FileNotFoundError:
  print('文件不存在')
except TypeError:
  print('类型错误')

print('--- 程序执行到了这里 ---')
```

**方式二**

```python
try:
  str = "str" + 0
  open('xxx.py', 'r')
except (FileNotFoundError, TypeError):
  print('程序发生错误')

print('--- 程序执行到了这里 ---')
```

### 捕获所有异常

在知道函数可能抛出异常类型的情况下，我们应该针对每种类型进行处理。如果不确定会发生什么异常呢？任何异常都可以被Exception类型捕获。

因为Exception是所有异常类的基类。FileNotFoundError和TypeError都继承自Exception。

```python
try:
  str = "str" + 0
  open('xxx.py', 'r')
except Exception:
  print('程序发生错误')

print('--- 程序执行到了这里 ---')
```

### 捕获异常信息

```python
try:
  str = "str" + 0
  open('xxx.py', 'r')
except Exception as result:
  print(result) # can only concatenate str (not "int") to str

print('--- 程序执行到了这里 ---')
```

通过`as`可以将异常赋值给result变量。

### else

```python
try:
  # open('xxx.py', 'r')
  print('---- 程序正常执行 ----')
except Exception as result:
  print(result) # [Errno 2] No such file or directory: 'xxx.text'
else:
  print('---- 程序没有异常 ---')
print('--- 程序执行到了这里 ---')
```

如果没有捕获到异常，会执行else中的代码。

### finally

无论有没有发生异常，都会执行finally中的代码。如关闭文件等操作。

```python
import time
try:
    f = open('test.txt')
    try:
        while True:
            content = f.readline()
            if len(content) == 0:
                break
            time.sleep(2)
            print(content)
    except:
        [[如果在读取文件的过程中，产生了异常，那么就会捕获到]]
        [[比如]] 按下了 ctrl+c
        pass
    finally:
        f.close()
        print('关闭文件')
except:
    print("没有这个文件")
```

## raise 抛出异常

### 向上抛出异常

如果某断代码可能放生异常，可以通过`raise`向上抛出异常，暂不处理异常，留给调用者处理。

```python
def read_text():
  raise open('xxx.text', 'r')

try:
  read_text()
except Exception as result:
  print(result) # [Errno 2] No such file or directory: 'xxx.text'

print('--- 程序执行到了这里 ---')
```

### 抛出自定义异常

所有的异常类都必须继承Exception，通过`raise`抛出异常。

```python
class ShortInputException(Exception):
    '''自定义的异常类'''
    def __init__(self, length, atleast):
        [[super]]().__init__()
        self.length = length
        self.atleast = atleast

def main():
    try:
        s = input('请输入 --> ')
        if len(s) < 3:
            # raise引发一个你定义的异常
            raise ShortInputException(len(s), 3)
    except ShortInputException as result:#x这个变量被绑定到了错误的实例
        print('ShortInputException: 输入的长度是 %d,长度至少应是 %d'% (result.length, result.atleast))
    else:
        print('没有异常发生.')

main()
```

