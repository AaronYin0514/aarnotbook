---
tags: [python, if]
---
# 条件判断

## 条件判断

```python
x = input('请输入第一个数字(x): ')
y = input('请输入第二个数字(y): ')
operator = input('请输入一个运算符: ')

x = int(x)
y = int(y)
result = None

if operator == '+':
  result = x + y
elif operator == '-':
  result = x - y
elif operator == '*':
  result = x * y
elif operator == '/':
  result = x / y
elif operator == '%':
  result = x % y
elif operator == '**':
  result = x ** y

if result == None:
  print('未能识别您输入的内容')
else:
  print('%d %s %d = %d'%(x, operator, y, result))
```

测试:

```
请输入第一个数字(x): 5
请输入第二个数字(y): 3
请输入一个运算符: **
5 ** 3 = 125

请输入第一个数字(x): 5
请输入第二个数字(y): 3
请输入一个运算符: ***
未能识别您输入的内容
```

## None、0、空字符串、空数组、空字典都相当于`False`

```python
if None:
  print('...')

if "":
  print('...')

if 0:
  print('...')

if []:
  print('...')

if {}:
  print('...')
```

所以None、0、空字符串、空数组、空字典都相当于False。

## None判断使用`is` `is not`

```python
a = None
if a is None:
	print('a is None')

b = 'str'
if b is not None:
	print('b is not None')
```

![[py_base_0300运算符#三目运算符]]