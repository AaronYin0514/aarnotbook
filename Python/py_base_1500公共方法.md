---
tags: [python]
---
# 公共方法

## 运算符

| 运算符 | Python 表达式 |	结果 |	描述 |	支持的数据类型 |
| ---- | ---- | ---- | ---- | ---- |
| + |	[1, 2] + [3, 4] |	[1, 2, 3, 4] |	合并 | 字符串、列表、元组 |
| * |	'Hi!' * 4 |	['Hi!', 'Hi!', 'Hi!', 'Hi!'] |	复制 | 字符串、列表、元组 |
| in | 3 in (1, 2, 3) |	True | 元素是否存在 | 字符串、列表、元组、字典 |
| not in | 4 not in (1, 2, 3) |	True | 元素是否不存在 | 字符串、列表、元组、字典 |

### +

```python
>>> "hello " + "itcast"
'hello itcast'
>>> [1, 2] + [3, 4]
[1, 2, 3, 4]
>>> ('a', 'b') + ('c', 'd')
('a', 'b', 'c', 'd')
```

### *

```python
>>> 'ab'*4
'ababab'
>>> [1, 2]*4
[1, 2, 1, 2, 1, 2, 1, 2]
>>> ('a', 'b')*4
('a', 'b', 'a', 'b', 'a', 'b', 'a', 'b')
```

### in

```python
>>> 'itc' in 'hello itcast'
True
>>> 3 in [1, 2]
False
>>> 4 in (1, 2, 3, 4)
True
>>> "name" in {"name":"Delron", "age":24}
True
```

> 注意，in在对字典操作时，判断的是字典的键

## python内置函数

Python包含了以下内置函数

| 序号 | 方法 |	描述 |
| --- | --- | --- |
| 1 | cmp(item1, item2) | 比较两个值 |
| 2 | len(item) | 计算容器中元素个数 |
| 3 | max(item) | 返回容器中元素最大值 |
| 4 | min(item) | 返回容器中元素最小值 |
| 5 | del(item) | 删除变量 |

### cmp

```python
>>> cmp("hello", "itcast")
-1
>>> cmp("itcast", "hello")
1
>>> cmp("itcast", "itcast")
0
>>> cmp([1, 2], [3, 4])
-1
>>> cmp([1, 2], [1, 1])
1
>>> cmp([1, 2], [1, 2, 3])
-1
>>> cmp({"a":1}, {"b":1})
-1
>>> cmp({"a":2}, {"a":1})
1
>>> cmp({"a":2}, {"a":2, "b":1})
-1
```

> 注意：cmp在比较字典数据时，先比较键，再比较值。

 ### len

```python
>>> len("hello itcast")
12
>>> len([1, 2, 3, 4])
4
>>> len((3,4))
2
>>> len({"a":1, "b":2})
2
```

> 注意：len在操作字典数据时，返回的是键值对个数。

### max

```python
>>> max("hello itcast")
't'
>>> max([1,4,522,3,4])
522
>>> max({"a":1, "b":2})
'b'
>>> max({"a":10, "b":2})
'b'
>>> max({"c":10, "b":2})
'c'
```

### del

del有两种用法，一种是`del`加空格，另一种是`del()`

```python
>>> a = 1
>>> a
1
>>> del a
>>> a
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'a' is not defined
>>> a = ['a', 'b']
>>> del a[0]
>>> a
['b']
>>> del(a)
>>> a
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'a' is not defined
```

### key参数

传入一个key函数作为比较算法

```python
ps = [
	{"name": "张三","age": 10},
	{"name": "李四","age": 12},
	{"name": "王五","age": 9},
]

m = max(ps, key=lambda x: x["age"])
print(m) # {'name': '李四', 'age': 12}
```

### getattr

`getattr`函数用于返回一个对象属性值。

```python
getattr(object, name[, default])
```

- object – 对象。
- name – 字符串，对象属性。
- default – 默认返回值，如果不提供该参数，在没有对应属性时，将触发 AttributeError。

## 多维列表/元祖访问的示例

```python
>>> tuple1 = [(2,3),(4,5)]
>>> tuple1[0]
(2, 3)
>>> tuple1[0][0]
2
>>> tuple1[0][2]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: tuple index out of range
>>> tuple1[0][1]
3
>>> tuple1[2][2]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
>>> tuple2 = tuple1+[(3)]
>>> tuple2
[(2, 3), (4, 5), 3]
>>> tuple2[2]
3
>>> tuple2[2][0]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'int' object is not subscriptable
```