---
tags: [python, set]
---
# 集合

## 定义集合

```python
languages = {'Python', 'Swift', 'OC', 'Ruby'}
print(languages)

# 定义空集合
emptySet = set()
print(emptySet)
```

## 访问

```python
lan1 = {'Python', 'Swift', 'OC', 'Ruby'}

for item in lan1:
    print(item)
```

## 元素个数

```python
languages = {'Python', 'Swift', 'OC', 'Ruby'}
length = len(languages)
print(length)
```

## 添加元素

### add

`add`将元素添加到集合中，如果元素已存在，则不进行任何操作。

```python
languages = {'Python', 'Swift', 'OC', 'Ruby'}
# add添加元素
languages.add("Java")

print(languages)
```

### update

`update`也可以添加元素，且参数可以是列表，元组，字典等

```javascript
languages = {'Python', 'Swift', 'OC', 'Ruby'}
# update添加元素
languages.update(["Java", "JavaScript"])

print(languages)
```

## 移除元素

### remove

`remove`将元素从集合中移除，如果元素不存在，则会发生错误。

```python
languages = {'Python', 'Swift', 'OC', 'Ruby'}
# remove移除元素
languages.remove("Python")

print(languages)
```

### discard

`discard`移除集合中的元素，且如果元素不存在，不会发生错误

```python
languages = {'Python', 'Swift', 'OC', 'Ruby'}
# discard移除元素
languages.discard("Python")
languages.discard("Java")

print(languages)
```

### pop

`pop`随机删除一个元素

```python
languages = {'Python', 'Swift', 'OC', 'Ruby'}
# pop随机删除一个元素
languages.pop()

print(languages)
```

### clear

`clear`清空集合

```python
languages = {'Python', 'Swift', 'OC', 'Ruby'}
# clear清空集合
languages.clear()

print(languages)
```

## 判断元素是否存在

```python
languages = {'Python', 'Swift', 'OC', 'Ruby'}

if ("Python" in languages):
    print("Python存在")

if ("Java" not in languages):
    print("Java不存在")
```

## 集合操作

并集、交集、补集等

```python
lan1 = {'Python', 'Swift', 'OC', 'Ruby'}
lan2 = {'Python', 'Java', 'JavaScript', 'Ruby'}
# 并集
lan3 = lan1 | lan2
print(lan3) # {'Java', 'Ruby', 'Swift', 'OC', 'Python', 'JavaScript'}
# 交集
lan4 = lan1 & lan2
print(lan4) # {'Python', 'Ruby'}
# 属于lan1，不属于lan2
lan5 = lan1 - lan2
print(lan5) # {'Swift', 'OC'}
# 不同时属于lan1和lan2
lan6 = lan1 ^ lan2
print(lan6) # {'Java', 'Swift', 'OC', 'JavaScript'}
```

## 集合转数组

```pyhton
s=set('123456')

```