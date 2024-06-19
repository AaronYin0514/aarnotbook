---
tags: [python, copy]
---
# 深拷贝 与 浅拷贝

## is 与 ==

```python
a = [1, 2, 3]
b = a

print(a == b) # True
print(a is b) # True

print(id(a)) # 4374110464
print(id(b)) # 4374110464


c = [1, 2, 3]
d = [1, 2, 3]

print(c == d) # True
print(c is d) # False

print(id(c)) # 4377485440
print(id(d)) # 4377488064
```

* `is`是比较两个引用是否指向同一对象。
* `==`是比较两个对象值是否相等。

## 赋值

```python
a = [1, 2, 3]
b = a

print(id(a)) # 4413382464
print(id(b)) # 4413382464

a.append(4)
print(a) # [1, 2, 3, 4]
print(b) # [1, 2, 3, 4]
```

## copy 浅拷贝

```python
import copy

a = [1, 2, 3]
b = copy.copy(a)
c = copy.deepcopy(a)

print(id(a)) # 4351761408
print(id(b)) # 4351119104
print(id(c)) # 4351758656

a.append(4)
b.append(5)
c.append(6)
print(a) # [1, 2, 3, 4]
print(b) # [1, 2, 3, 5]
print(c) # [1, 2, 3, 6]
```

浅拷贝不同于直接赋值，它会copy一个新的对象出来。可以看到b的地址与a不同。但是copy虽然会拷贝对象，但是不会去遍历对象内部元素。这个例子并没有看出copy与deepcopy的区别。那看一下深拷贝的例子。

## deepcopy 深拷贝

```python
import copy

a = [1, [2, 3]]
b = copy.copy(a)
c = copy.deepcopy(a)

print(id(a)) # 4521832064
print(id(b)) # 4522471680
print(id(c)) # 4521428160

a[1].append(4)

print(a) # [1, [2, 3, 4]]
print(b) # [1, [2, 3, 4]]
print(c) # [1, [2, 3]]

print(id(a[1])) # 4501826432
print(id(b[1])) # 4501826432
print(id(c[1])) # 4501839424
```

虽然copy拷贝了对象a（b与a地址不同），但是并没有拷贝a的全部元素，即a[1]与b[1]仍然指向相同的对象。而append拷贝了所有a对象的元素。

## 拷贝不可变类型数据

```python
import copy

a = (1, 2, 3)

b = copy.copy(a)
c = copy.deepcopy(a)

print(id(a)) # 4413476288
print(id(b)) # 4413476288
print(id(c)) # 4413476288
```

可以看出拷贝不可变类型数据相当于直接赋值。





