---
tags: [python, input]
---
# 输入

## input

在Python中，`input`函数获取键盘输入内容，需要注意的是，获取到的数据是**字符串**类型

```python
x = input('请输入一个数字(x): ')
y = input('请输入第二个数字(y): ')
print('x + y = %d'%(int(x) + int(y)))
```

测试：

```
请输入一个数字(x): 19
请输入第二个数字(y): 21
x + y = 40
```

Tip：

> 在Python2中与input等价的函数是`raw_input`
> 在Python2中的`input`函数输入内容会被当作表达式处理
> 在Python3中只有`input`函数，没有`raw_input`函数了

