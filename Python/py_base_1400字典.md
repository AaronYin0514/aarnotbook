---
tags: [python, directory]
---
# 字典

## 定义字典 及 访问

```python
zhang = {"name": "张三", "age": 18, "height": 180}
print(zhang["name"]) # 张三
print(zhang["age"]) # 18
print(zhang["height"]) # 180
```

## 增

```python
zhang = {"name": "张三", "age": 18, "height": 180}
print(zhang) # {'name': '张三', 'age': 18, 'height': 180}
zhang["qq"] = "123456"
print(zhang) # {'name': '张三', 'age': 18, 'height': 180, 'qq': '123456'}
```

## 删

### del

```python
zhang = {"name": "张三", "age": 18, "height": 180}
print(zhang) # {'name': '张三', 'age': 18, 'height': 180}
del zhang["age"]
print(zhang) # {'name': '张三', 'height': 180}
```

### clear

```python
zhang = {"name": "张三", "age": 18, "height": 180}
print(zhang) # {'name': '张三', 'age': 18, 'height': 180}
zhang.clear()
print(zhang) # {}
```

## 改

```python
zhang = {"name": "张三", "age": 18, "height": 180}
print(zhang) # {'name': '张三', 'age': 19, 'height': 180}
zhang["age"] = 19
print(zhang)
```

## 查

### 通过key访问value

访问不存在的key会报错

```python
zhang = {"name": "张三", "age": 18, "height": 180}
print(zhang["name"]) # 张三
print(zhang["qq"]) # 报错
```

### in / not in - 判断字典中是否包含key

```python
zhang = {"name": "张三", "age": 18, "height": 180}
print("age" in zhang) # True
print('qq' in zhang) # False
print('wechat' not in zhang) # True
```

### get

如果`get`的key不存在，返回**None**

```python
zhang = {"name": "张三", "age": 18, "height": 180}
print(zhang.get('name')) # 张三
print(zhang.get('qq')) # None
```

## len - 获取字典长度

```python
zhang = {"name": "张三", "age": 18, "height": 180}
print(len(zhang)) # 3
```

## keys - 获取所有的key

```python
zhang = {"name": "张三", "age": 18, "height": 180}
print(zhang.keys()) # dict_keys(['name', 'age', 'height'])
```

## values - 获取所有的values

```python
zhang = {"name": "张三", "age": 18, "height": 180}
print(zhang.values()) # dict_values(['张三', 18, 180])
```

## items - 获取所有键值对

```python
zhang = {"name": "张三", "age": 18, "height": 180}
print(zhang.items()) # dict_items([('name', '张三'), ('age', 18), ('height', 180)])
```

## 遍历字典

```python
zhang = {"name": "张三", "age": 18, "height": 180}
for key, value in zhang.items():
  print('key=%s, value=%s'%(key, str(value)))
```

## 拷贝字典(浅拷贝)

```python
a = {'name': '张三', 'age': 100, 'pp': [1, 2, 4]}
b = a.copy()

a['pp'].append(5)

print(id(a)) # 4302864832
print(id(b)) # 4302865024

print(a) # {'name': '张三', 'age': 100, 'pp': [1, 2, 4, 5]}
print(b) # {'name': '张三', 'age': 100, 'pp': [1, 2, 4, 5]}
```