---
tags: [python, string]
---
# 字符串常见操作

## find & rfind - 获取某子字符串索引

检测 某字符串 是否包含在一个字符串中，如果是返回开始的索引值，否则返回-1

> 方法前面有r表示从右边开始查找

```python
str = 'Life is shot, you need Python.'

index = str.find('you')
print(index) # 24

index = str.rfind('o')
print(index) # 27

index = str.find('Ruby')
print(index) # -1
```

## index & rindex - 获取某子字符串索引

index跟`find`方法一样，只不过在没有找到时会报一个异常。

```python
str = 'Life is shot, you need Python.'

index = str.index('you')
print(index) # 24

index = str.rindex('o')
print(index) # 27

index = str.index('Ruby')
print(index) # 报错
```

## count - 查找某字符串出现次数

返回某一字符串在另一个字符串中出现的次数

```python
str = 'Life is shot, you need Python.'

c = str.count('o')
print(c) # 3
```

## replace - 替换

将字符串中某一断字符串替换成指定字符串

```python
str = 'Life is shot, you need Java. Life is shot, you need Java.'

str1 = str.replace('Java', 'Python')
print(str1)

# 1表示替换最多不超过1次，不传全部替换
str2 = str.replace('Java', 'Python', 1)
print(str2)
```

结果

```
Life is shot, you need Python. Life is shot, you need Python.
Life is shot, you need Python. Life is shot, you need Java.
```

## split & splitlines - 分割

以指定字符串为分割符，分割字符串返回数组

```python
str1 = 'Life is shot, you need Python.'
# 以空格分割
arr1 = str1.split(' ')
print(arr1) # ['Life', 'is', 'shot,', 'you', 'need', 'Python.']

str2 = 'Life is shot\nyou need Python.'
# 以换行符分割
arr2 = str2.splitlines()
print(arr2) # ['Life is shot', 'you need Python.']
```

## partition & rpartition - 将字符串分割成3部分

返回结果为元组类型

```python
str = 'Life is shot, you need Python.'

print(str.partition('you')) # ('Life is shot, ', 'you', ' need Python.')
print(str.rpartition('o')) # ('Life is shot, you need Pyth', 'o', 'n.')
```

## capitalize & title - 首字母大写

```python
str = 'life is shot, you need Python.'

# 首字母大写
str1 = str.capitalize()
print(str1) # Life is shot, you need python.

# 每个单词的首字母都大写
str2 = str.title()
print(str2) # Life Is Shot, You Need Python.
```

## startswith & endswith - 前缀 后缀

```python
fileName = 'test.py'

if fileName.startswith('test'):
  print('这是一个测试文件')

if fileName.endswith('py'):
  print('这是一个python脚本文件')
```

## lower & upper - 字符串转小写/大写

```python
command = input('是否确认删除该文件?(yes, no): ')
if command.upper() == 'YES':
  print('文件已删除')
elif command.upper() == 'NO':
  print('没有删除你的文件')
else:
  print('未能识别指令')

if command.lower() == 'yes':
  print('文件已删除')
elif command.lower() == 'no':
  print('没有删除你的文件')
else:
  print('未能识别指令')
```

测试

```
是否确认删除该文件?(yes, no): Yes
文件已删除
文件已删除
```

## ljuest & rjuest & center - 居左/居右/居中 对齐字符串

#### 居右对齐

```python
str1 = 'life is shot'
str2 = 'you need Python.'

strr1 = str1.rjust(30)
strr2 = str2.rjust(30)

print(strr1)
print(strr2)
```

结果

```
                 life is shot
              you need Python. 
```

#### 居左对齐

```python
str1 = 'life is shot'
str2 = 'you need Python.'

strl1 = str1.ljust(30)
strl2 = str2.ljust(30)

print(strl1)
print(strl2)
```

结果

```
life is shot                  
you need Python.                
```

#### 居中对齐

```python
str1 = 'life is shot'
str2 = 'you need Python.'

strc1 = str1.center(30)
strc2 = str2.center(30)

print(strc1)
print(strc2)
```

结果

```             
         life is shot         
       you need Python.  
```

## lstrip & rstrip & strip - 去掉字符串左边/右边/两边空白字符

```python
str = '       Life is shot, you need Python.       '

str1 = str.lstrip()
str2 = str.rstrip()
str3 = str.strip()

print(str1) # Life is shot, you need Python.
print(str2) #        Life is shot, you need Python.
print(str3) # Life is shot, you need Python.
```

## isalpha & isdigit & isalnum - 判断是纯字母/纯数字/混合字符串

```python
str1 = '12 345 '
str2 = 'Hello, Python'
str3 = '123 Hello, Python'

if str1.isdigit:
  print('是数字')

if str2.isalpha:
  print('是字母')

if str3.isalnum:
  print('是混合')
```

## isspace - 判断是否是纯空格

```python
str = '    '
print(str.isspace()) # True
```

## join - 拼接

```python
str1 = 'P'
str2 = 'H' * 3

print(str1.join(str2)) # HPHPH

arr = ['Life', 'is', 'shot,', 'you', 'need', 'Python.']
str = ' '
print(str.join(arr)) # Life is shot, you need Python.
```



