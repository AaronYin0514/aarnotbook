# zip

`zip`函数将多个序列（包括：**列表**、**元组**、**字典**、**集合**、**字符串**）对象中对应的元素打包成一个个**元组**，返回**zip**对象。

## 基本使用

### `zip`函数 & zip对象遍历

```python
list_a = ['a', 'b', 'c']
list_b = [1, 2, 3]

z = zip(list_a, list_b)

print(z) # <zip object at 0x100d03700>

# 遍历
print([x for x in z]) 
# [('a', 1), ('b', 2), ('c', 3)]

str_a = "python"
str_b = "shell"

print([x for x in zip(str_a, str_b)]) 
# [('p', 's'), ('y', 'h'), ('t', 'e'), ('h', 'l'), ('o', 'l')]
```

### zip类型转换

```python
list_a = ['a', 'b', 'c']
list_b = [1, 2, 3]

# zip --> list
print(list(zip(list_a, list_b))) 
# [('a', 1), ('b', 2), ('c', 3)]

# zip --> dict
print(dict(zip(list_a, list_b))) 
# {'a': 1, 'b': 2, 'c': 3}
```

### 参数个数不同

返回列表长度与最短的对象相同

```python
list_a = [1, 2, 3]
list_b = [4, 5, 6, 7, 8]

print(list(zip(list_a, list_b)))
# [(1, 4), (2, 5), (3, 6)]
```

### \*号操作符

利用 \* 号操作符，可以将元组解压为列表

与`zip`相反，`zip(*)`可理解为解压，将元组解压为列表

```python
list_a = [1, 2, 3]
list_b = [4, 5, 6]

list1, list2 = zip(*zip(list_a, list_b))

print(list(list1)) # [1, 2, 3]
print(list(list2)) # [4, 5, 6]
```








