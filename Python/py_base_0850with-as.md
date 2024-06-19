---
tags: [python, with, with-as]
---

# with-as

## 什么是with语句？

with 语句是从 Python 2.6 开始引入的一种与异常处理相关的功能。with 语句适用于对资源进行访问的场合，确保不管使用过程中是否发生异常都会执行必要的“清理”操作，释放资源，比如文件使用后自动关闭、线程中锁的自动获取和释放等。

![](https://www.biaodianfu.com/wp-content/uploads/2021/09/python-with.jpg)

with语句是一个新的控制流结构，其基本结构为：

```python
with expression [as variable]:
	with-block
```

一个很好的例子是文件处理，你需要获取一个文件句柄，从文件中读取数据，然后关闭文件句柄。如果不用`with`语句，代码如下：

```python
file = open("foo.txt")
data = file.read()
file.close()
```

这里有两个问题：

- 可能忘记关闭文件句柄
- 文件读取数据发生异常，没有进行任何处理

下面是处添加了异常处理的代码：

```python
file = open("foo.txt")
try:
	data = file.read()
finally:
	file.close()
```

虽然这段代码运行良好，但是太冗长了。这时候就是`with`一展身手的时候了。除了有更优雅的语法，`with`还可以很好的处理上下文环境产生的异常。下面是`with`版本的代码：

```python
with open("foo.txt") as file:
	data = file.read()
```

## with语句是如何工作的？

Python对`with`的处理还聪明。基本思想是`with`所求值的对象必须有一个`enter()`方法，一个`exit()`方法。紧跟`with`后面的语句被求值后，返回对象的`enter()`方法被调用，这个方法的返回值将被赋值给`as`后面的变量。当`with`后面的代码块全部被执行完之后，将调用前面返回对象的`exit()`方法。

下面例子可以具体说明`with`如何工作：

```python
class Sample:
	def __enter__(self):
		print("In __enter__()")
		return "Foo"
	
	def __exit__(self, type, value, trace):
		print("In __exit__()")

def get_sample():
	return Sample()

with get_sample() as sample:
	print("sample:", sample)
```

执行后的输出内容为：

```
In __enter__()
sample: Foo
In __exit__()
```

具体流程：

- `enter()`方法被执行
- `enter()`方法返回的值 – 这个例子中是**Foo**，赋值给变量**sample**
- 执行代码块，打印变量**sample**的值为**Foo**
- `exit()`方法被调用

`with`真正强大之处是它可以处理异常。可能你已经注意到`Sample`类的`exit`方法有三个参数：`val`, `type` 和 `trace`。 这些参数在异常处理中相当有用。我们来改一下代码，看看具体如何工作的。

```python
class Sample:
	def __enter__(self):
		return self
	
	def __exit__(self, type, value, trace):
		print("type:", type)
		print("value:", value)
		print("trace:", trace)

def do_something(self):
	bar = 1 / 0
	return bar

with Sample() as sample:
	sample.do_something()
```


输出结果为：

```
Traceback (most recent call last):
File "test.py", line 16, in <module>
sample.do_something()
File "test.py", line 11, in do_something
bar = 1 / 0
ZeroDivisionError: division by zero
type: <class 'ZeroDivisionError'>
value: division by zero
trace: <traceback object at 0x0000029739A7F508>
```


这个例子中，`with`后面的`get_sample()`变成了`Sample()`。这没有任何关系，只要紧跟with后面的语句所返回的对象有`enter()`和`exit()`方法即可。此例中，`Sample()`的`enter()`方法返回新创建的`Sample`对象，并赋值给变量`sample`。

实际上，在`with`后面的代码块抛出任何异常时，`exit()`方法被执行。正如例子所示，异常抛出时，与之关联的`type`，`value`和`stack trace`传给`exit()`方法，因此抛出的`ZeroDivisionError`异常被打印出来了。开发库时，清理资源，关闭文件等等操作，都可以放在`exit`方法当中。

因此，Python的`with`语句是提供一个有效的机制，让代码更简练，同时在异常产生时，清理工作更简单。

## 参考资料

- [Python 中 with用法及原理](https://blog.csdn.net/u012609509/article/details/72911564)
- [# With As 流程控制](https://medium.com/%E7%A8%8B%E5%BC%8F%E4%B9%BE%E8%B2%A8/python-with-as-%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6-bc5850dee667)