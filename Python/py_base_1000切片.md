---
tags: [python, slice]
---
## 切片

切片是指对操作的对象截取其中一部分的操作。字符串、列表、元组都支持切片操作。

切片的语法：**起始:结束:步长**

> 注意：选取的区间属于左闭右开型，即从"起始"位开始，到"结束"位的前一位结束（不包含结束位本身)。

```python
str = 'Life is shot, you need Python'

# 取出"shot"
print(str[8:12])

# 取出"you need Python"
# 省略代表到达最后一个
print(str[14:])

# 打印最后一个字符
print(str[-1])

# 从后边数 取出Python
print(str[-6:])

# 取出need
print(str[18:-7])

# 设置步长
print(str[0:4:2])

# 翻转字符串
print(str[::-1])
```