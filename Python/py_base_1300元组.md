---
tags: [python, tuple]
---
# 元组

Python的元组与列表类似，不同之处在于**元组的元素不能修改**。

## 定义元组

```python
languages = ('Python', 'Swift', 'Ruby')
print(languages) # ('Python', 'Swift', 'Ruby')
```

> 当元组只有一个元素时，需要加`,`，避免语意上的冲突

```python
t1 = (1) 
print(t1) # 1

t2 = (1,)
print(t2) # (1,)
```

例子中，t1是一个数字，t2才是元组

## 访问元组

### 通过索引访问

```python
languages = ('Python', 'Swift', 'Ruby')
print(languages[0]) # Python
```

### 拆包

```python
languages = ('Python', 'Swift', 'Ruby')
p, s, r = languages
print(p) # Python
print(s) # Swift
print(r) # Ruby
```

## 应用

### 交换两个变量的值

```python
a = 5
b = 4
print('a=',a,'b=',b) # a= 5 b= 4

a, b = b, a
print('a=',a,'b=',b) [[a]]= 4 b= 5
```