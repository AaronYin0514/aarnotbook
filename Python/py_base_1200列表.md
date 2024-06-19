---
tags: [python, array]
---
# 列表

## 定义列表

```python
languages = ['Python', 'Swift', 'OC', 'Ruby']
```

## 访问列表

```python
languages = ['Python', 'Swift']
print(languages[0]) # Python
print(languages[1]) # Swift
```

## 增

### append - 添加元素

```python
languages = ['Python', 'Swift', 'OC', 'Ruby']
print(languages) # ['Python', 'Swift', 'OC', 'Ruby']
languages.append('JS')
print(languages) # ['Python', 'Swift', 'OC', 'Ruby', 'JS']
```

### insert - 插入元素

```python
languages = ['Python', 'Swift', 'OC', 'Ruby']
print(languages) # ['Python', 'Swift', 'OC', 'Ruby']
languages.insert(1, 'JS')
print(languages) # ['Python', 'JS', 'Swift', 'OC', 'Ruby']
```

### extend - 拼接两个数组

```python
languages = ['Python', 'Swift']
print(languages) # ['Python', 'Swift']
languages1 = ['OC', 'Ruby']
languages.extend(languages1)
print(languages) # ['Python', 'Swift', 'OC', 'Ruby']
```

## 删

### pop - 删除最后一个元素

```python
languages = ['Python', 'Swift']
print(languages) # ['Python', 'Swift']
languages.pop()
print(languages) # ['Python']
```

### remove - 删除某一个元素

```python
languages = ['Python', 'Swift']
print(languages) # ['Python', 'Swift']
languages.remove('Swift')
print(languages) # ['Python']
```

### del - 删除索引元素

```python
languages = ['Python', 'Swift']
print(languages) # ['Python', 'Swift']
del languages[1]
print(languages) # ['Python']
```

## 改

```python
languages = ['Python', 'swift']
print(languages) # ['Python', 'swift']
languages[1] = 'Swift'
print(languages) # ['Python', 'Swift']
```

## 查找索引

```python
languages = ['Python', 'Swift', 'OC', 'Ruby']

print(languages.index("Swift"))

try:
    print(languages.index("JavaScript"))
except:
    print("JavaScript没有找到")
```

## in / not in - 查

```python
languages = ['Python', 'Swift']

if 'Swift' in languages:
  print('Swift在列表中')

if 'Ruby' not in languages:
  print('Ruby不在列表中')
```

## len - 获取列表长度

```python
languages = ['Python', 'Swift']
print(len(languages)) # 2
```

## 遍历

### forin遍历

```python
languages = ['Python', 'Swift']
for l in languages:
  print(l)
```

### enumerate - 带索引遍历

很多时候，在遍历列表时，你想同时拿到索引和值

```python
languages = ['Python', 'Swift']
for index, value in enumerate(languages):
  print('index=%d value=%s'%(index, value))
```

## sort - 排序

`sort`默认是将列表按**升序**排列，`reverse=True`参数表示**降序**

```python
nums = [1, 4, 2, 3]
print(nums) # [1, 4, 2, 3]
nums.sort()
print(nums) # [1, 2, 3, 4]
nums.sort(reverse=True)
print(nums) # [4, 3, 2, 1]
```

## reverse - 翻转数组

```python
nums = [1, 4, 2, 3]
print(nums) # [1, 4, 2, 3]
nums.reverse()
print(nums) # [3, 2, 4, 1]
```

## 运算符

### + 拼接两个数组

```python
languages = ['Python', 'Swift']
languages1 = ['OC', 'Ruby']
ls = languages + languages1
print(ls) # ['Python', 'Swift', 'OC', 'Ruby']
```

### * 快速创建一个重复元素的数组

```python
arr = [1, 3]
print(arr) # [1, 3]
arr = arr * 3
print(arr) # [1, 3, 1, 3, 1, 3]
```

## 打乱元素顺序

⚠️ 不返回新数组对象，打乱原数组元素

```python
# shuffle()使用样例
import random

x = [i for i in range(10)]
print(x) # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
random.shuffle(x)
print(x) #[2, 5, 4, 8, 0, 3, 7, 9, 1, 6]
```

# range

range用法与切片类似。`range(起始位置, 结束位置, 步长)`

```python
for i in range(2, 5):
  print(i)

# 结果：2 3 4

for i in range(2, 5, 2):
  print(i)

# 结果：2 4

for i in range(5):
  print(i)

# 结果：0 1 2 3 4
```

# 列表推导式

### 创建1到17的数组

```python
a = [i for i in range(1, 18)]
# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17]
```

### 创建1到17中偶数的数组

```python
a = [i for i in range(1, 18) if i % 2 == 0]
# [2, 4, 6, 8, 10, 12, 14, 16]
```

### 创建坐标

```python
a = [(i, j) for i in range(3) for j in range(2)]
# [(0, 0), (0, 1), (1, 0), (1, 1), (2, 0), (2, 1)]
```

## map

map() 会根据提供的函数对指定序列做映射。

第一个参数 function 以参数序列中的每一个元素调用 function 函数，返回包含每次 function 函数返回值的新列表。

```python
languages = ['Python', 'Swift', 'OC', 'Ruby']
def lower(str):
    return str.lower()

i = map(lower, languages)
l = list(i)
print(l)
```

使用匿名函数

```python
l = [1,2,3,4]
iterator = map(lambda item: item ** 2, l)
l = list(iterator)
print(l)
```

## reduce

reduce() 函数会对参数序列中元素进行累积。

函数将一个数据集合（链表，元组等）中的所有数据进行下列操作：用传给 reduce 中的函数 function（有两个参数）先对集合中的第 1、2 个元素进行操作，得到的结果再与第三个数据用 function 函数运算，最后得到一个结果。

```
reduce(function, iterable[, initializer])
```

* function -- 函数，有两个参数
* iterable -- 可迭代对象
* initializer -- 可选，初始参数

```python
from functools import reduce

l = [1,2,3,4]
result = reduce(lambda x,y: x + y, l, 0)
print(result)
```

## filter

filter() 函数用于过滤序列，过滤掉不符合条件的元素，返回由符合条件元素组成的新列表。

该接收两个参数，第一个为函数，第二个为序列，序列的每个元素作为参数传递给函数进行判断，然后返回 True 或 False，最后将返回 True 的元素放到新列表中。

```python
l = [1,2,3,4]
i = filter(lambda x: x % 2 == 1, l)
l = list(i) # [1, 3]
print(l)
```

