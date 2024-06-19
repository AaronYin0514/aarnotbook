---
tags: python, 多进程
---

# Fork

## 创建进程

### fork

在Unix/Linux/Mac环境中，Python通过`os`系统的`fork`函数创建**进程**。

```python
import os

rpid = os.fork()

if rpid < 0:
	print('调用失败')
elif rpid == 0:
	print('我是子进程%s，我的父进程是%s' % (os.getpid(), os.getppid()))
else:
	print("我是父进程%s，我的子进程是%s" % (os.getpid(), rpid))
```

> fork函数会返回两次值。调用`fork`函数，系统创建一个进程（称为父进程），并复制一份（称为子进程）。
> 分别在父进程和子进程中返回函数值。
> 子进程返回值为0，
> 父进程返回值是子进程的ID。

### 获取当前进程的ID

```python
os.getpid()
```

### 子进程获取父进程的ID

```python
os.getppid()
```

## 进程间数据独立

```python
import os
import time

num = 0

rpid = os.fork()

if rpid < 0:
	print('调用失败')
elif rpid == 0:
	num += 1
	print('我是子进程%s，num=%d' % (os.getpid(), num))
else:
time.sleep(1)
	num += 1
	print("我是父进程%s，num=%d" % (os.getpid(), num))
```

结果：

```
我是子进程70671，num=1
我是父进程70670，num=1
```




