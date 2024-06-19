---
tags: [python, file]
---
# 文件夹操作

## 创建文件夹

```python
import os
os.mkdir('mytest')
```

## 获取当前目录

```python
import os
d = os.getcwd()
print(d)
```

## 改变默认目录

```python
import os

print(os.getcwd())

os.chdir('mytest')

print(os.getcwd())
```

## 获取目录列表

```python
import os
fs = os.listdir('./')
print(fs)
```

## 删除文件夹

文件夹为空的情况

```python
import os
os.rmdir('mytest')
```

强制删除文件夹

```python
import shutil

shutil.rmtree(path, ignore_errors=True)
```

## 拷贝文件夹

```python
import shutil

# 使用copytree函数拷贝文件夹
shutil.copytree('source_folder', 'destination_folder')
```

## 文件夹路径

### 路径拼接

把两个路径合成一个时，不要直接拼字符串，而要通过os.path.join()函数，这样可以正确处理不同操作系统的路径分隔符。

```python
import os

path1 = "User"
path2 = "UserName"
path3 = "Document"
path4 = "test.py"

path = os.path.join(path1, path2, path3, path4)
print(path) # User/UserName/Document/test.py
```

### 获取文件名

```python
import os

path = "User/UserName/Document/test.py"
result = os.path.split(path)
print(result) # ('User/UserName/Document', 'test.py')
fileName = result[1]
print(fileName) # test.py
```

### 获取文件后缀

```python
path = "User/UserName/Document/test.py"
result = os.path.splitext(path)
print(result) # ('User/UserName/Document/test', '.py')
suffix = result[-1][1:]
print(suffix) # py
```