---
tags: [python, closure]
---
# 匿名函数

用lambda关键词能创建小型匿名函数。Lambda函数能接收任何数量的参数但只能返回一个表达式的值。

## 定义匿名函数

```python
lambda [arg1 [,arg2,.....argn]]:expression
```

## 数组排序

按name排序下面数组

```python
stus = [
    {"name":"zhangsan", "age":18}, 
    {"name":"lisi", "age":19}, 
    {"name":"wangwu", "age":17}
]
print(stus)

stus.sort(key= lambda x:x["name"])
print(stus)
```

结果

```
[{'name': 'zhangsan', 'age': 18}, {'name': 'lisi', 'age': 19}, {'name': 'wangwu', 'age': 17}]
[{'name': 'lisi', 'age': 19}, {'name': 'wangwu', 'age': 17}, {'name': 'zhangsan', 'age': 18}]
```

## 匿名函数作为函数参数

```python
def calculator(x, y, algorithm):
  return algorithm(x, y)

x = input('请输入第1个整数: ')
x = int(x)
y = input('请输入第2个整数: ')
y = int(y)
operator = input('请输入运算符: ')
if operator not in ['+', '-', '*', '/', '**']:
  print('未能识别的运算符!')
else:
  expression = 'lambda x, y: x ' + operator + ' y'
  result = calculator(x, y, eval(expression))
  print('结果是: %d' % result)
```

测试

```
请输入第1个整数: 5
请输入第2个整数: 3
请输入运算符: **
结果是: 125
```











