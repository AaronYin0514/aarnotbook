---
tags: [python]
---
# 编写第一个python程序

## python版本

现在常用的python版本是**python2**和**python3**。本文以python3为主。

## 终端python工具

打开终端，执行`python3`，在命令行输入`print("Hello,Python!")`，回车直接查看结果。

> 如果输入`python`，那么对应的版本是python2

```python
>>> print("Hello,Python!")
Hello,Python!
>>> 
```

## 编程工具

下载并安装[VS Code](https://code.visualstudio.com)

在插件中搜索python，安装插件，我们就可以方便的编写python代码了。

## 第一个python程序

1. 创建**hello_python.py**文件
2. 写入`print("Hello,Python!")`并保存

## 运行python

在终端中执行下面命令，就能看到结果啦

```shell
python3 hello_python.py
```

## python2中文支持问题

此时我们再添加一行代码:

```python
print("你好Python!")
```

这一个我们使用**python2**来执行程序

```shell
python hello_python.py
```

这时会看到程序发生错误，这是因为python2的默认编码问题导致的，我们需要在程序的第一行添加下面代码来指定程序编码。

```python
# -*- coding:utf-8 -*-
```

再次运行`python hello_python.py`就能看到正确的结果啦
