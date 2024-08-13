---
tags: [python, string]
---
# 字符串

## 格式转换 - 数值to字符串

```python
num1 = 100
num2 = 100.0

str1 = str(num1)
str2 = str(num2)

print(str1)
print(str2)
```

## 格式转换 - 字符串to数值

```python
str1 = '100'
str2 = '100.0'

num_from_str1 = int(str1)
num_from_str2 = float(str2)

print(num_from_str1)
print(num_from_str2)
```

## 字符串格式化

```python
name = '张三'
age = 18
height = 180
str_f = '我叫%s，我%d岁，我%dcm高'%(name, age, height)
print(str_f)
```

整数补0

```python
num = 1
str = "test%04d" % num

print(str) # test0001
```

### `f"xxx{name}xxx"`

```python
name = "Bob"
city = "New York"
message = f"Hello, my name is {name} and I live in {city}"
print(message)
```

字符串中包含花括号，使用**双重花括号**

```python
name = "Alice"
message = f"My name is {{name}}."
print(message)  # 输出：My name is {name}.
```

## 字符串运算

### +

```python
str1 = "张"
str2 = "三"
str3 = str1 + str2
print(str3) # 张三
```

### *

```python
str1 = "张"
str3 = str1 * 3
print(str3) # 张张张
```

## 获取字符串长度

```python
str = 'Hello,Python'
print(len(str))
```

### 下标索引

```python
str = "Hell,Python"
print(str[0])
print(str[len(str) - 1])
```

## 字符串遍历

### for in

```python
str = "Hello,Python!"

for c in str:
    print(c)
```

### 内置函数`range`

```python
str = "Hello,Python!"

for index in range(len(str)):
    print(str[index])
```

### 内置函数`enumerate`

```python
str = "Hello,Python!"

for index, c in enumerate(str):
    print("索引%i 字符%s"%(index, c))
```

### 内置函数`iter`

```python
str = "Hello,Python!"

for c in iter(str):
    print(c)
```