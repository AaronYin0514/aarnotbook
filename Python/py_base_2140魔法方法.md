---
tags: [python]
---

# 魔法方法

## `__getitem__`

这个方法返回与指定键相关联的值。

对序列来说（通常是列表），键应该是0~n-1的整数，其中n为序列的长度。

对映射来说（通常是字典），键可以是任何类型。

```python
class Tag:
    def __init__(self):
        self.change={'python':'Life is shot, you need python.'}
 
    def __getitem__(self, item):
        print('这个方法被调用')
        return self.change[item]
 
a=Tag()
print(a['python'])
```