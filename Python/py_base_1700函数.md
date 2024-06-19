---
tags: [python, func]
---
# 函数

## 定义函数 & 调用

```python
def sum2number(x, y):
  return x + y

result = sum2number(100, 200)
print(result) # 300
```

## 函数返回多个值 - 元组

```python
def square2number(x, y):
  return x * x, y * y

x = 10
y = 5
sx, sy = square2number(x, y)
print(sx) # 100
print(sy) # 25
```

## 函数缺省参数

缺省参数表示，如果未传入，则使用默认值。定义函数是，给形参一个默认值即表示缺省参数（如下面`power=2`）。

```python
# 幂运算
def my_pow(number, power=2):
  return number ** power

result = my_pow(10, 3)
print(result) # 1000

result = my_pow(10, power=3)
print(result) # 1000

result = my_pow(10) # 没有传入power参数，则表示power=2
print(result) # 100
```

> 注意：
> 1. 必选参数在前，缺省参数在后，否则Python的解释器会报错
> 2. 缺省参数需要指定为不可变类型

```python
def add_end(L=[]):
    L.append('END')
    return L

print(add_end([1, 2, 3])) # ✅ 结果正确 [1, 2, 3, 'END'] 
print(add_end(['x', 'y', 'z'])) # ✅ 结果正确 ['x', 'y', 'z', 'END'] 

print(add_end()) # ✅ 结果正确 ['END']
print(add_end()) # ❌ 结果错误 ['END', 'END']
print(add_end()) # ❌ 结果错误 ['END', 'END', 'END']
```

缺省参数指定为可变类型，虽然程序正常运行，但是在业务逻辑上很容易出错。

## 可变参数 - *args

在参数末尾添加`*args`表示可变参数，这样的函数在调用过程中，可以传入任意个参数（包括0个）。函数体内接收到`args`是传入参数形成的**元组**。

```python
def my_calculator(operator, *args):
  if operator == '+':
    result = 0
    for num in args:
      result += num
    return result
  elif operator == '*':
    result = 1
    for num in args:
      result *= num
    return result
  else:
    print('暂时不支持%s运算符'%operator)

r = my_calculator("+", 1, 2, 3, 4, 5)
print(r) # 15
```

## 关键字参数 - **kw

在参数末尾添加`**kw`表示可变参数，这样的函数在调用过程中，可以传入任意个命名参数（包括0个）。函数体内接收到`kw`是传入参数形成的**字典**。

```python
def my_infos(name, **kw):
  print("name: ", name)
  for k,v in kw.items():
    print("%s: %s"%(k, str(v)))

my_infos("张三", age=18, height=180, qq='123456')
```

结果：

```
name:  张三
age: 18
height: 180
qq: 123456
```

## 命名关键字参数

关键字参数`**kw`不同，命名关键字参数需要一个特殊分隔符`*`，`*`后面的参数被视为命名关键字参数。

> 1. 命名关键字参数必须传入参数名，否则调用将报错。
> 2. 命名关键字参数可以有缺省值。

```python
def my_infos(name, *, age=18, height):
  print(name, age, height)
  
my_infos("张三", height=180) # 张三 18 180
```

如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了：

```python
def my_infos(name, age, *args, city, job):
    print(name, age, args, city, job)
```

## 参数组合

> 注意⚠️  - 缺省参数/可变参数/关键字参数 虽然提高了函数的灵活性，但是也增加了函数的可读性，应该在合适的地方使用

```python
def f1(a, c=0, *args, **kw):
    print('a =', a, 'c =', c, 'args =', args, 'kw =', kw)

f1(10, 3, 'a', 'r', 'g', 's', k = 'k1', w = 'w1') # a = 10 c = 3 args = ('a', 'r', 'g', 's') kw = {'k': 'k1', 'w': 'w1'}
f1(10, k = 'k1', w = 'w1') # a = 10 c = 0 args = () kw = {'k': 'k1', 'w': 'w1'}
```

## 拆包 - 通过传入一个tuple和dict调用

调用时，在元组前面加`*`，在字典前面加`**`，传入参数

```python
def f1(a, c=0, *args, **kw):
    print('a =', a, 'c =', c, 'args =', args, 'kw =', kw)

args = [1, 2, 3]
kw = {'k': 'k2', 'w': 'w2'}
f1(*args, **kw) # a = 1 c = 2 args = (3,) kw = {'k': 'k2', 'w': 'w2'}
```