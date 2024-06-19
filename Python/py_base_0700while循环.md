---
tags: [python, while]
---
# while循环

求1～100之间偶数的和

```python
reslult = 0
i = 1
while i <= 100:
  if i % 2 == 0:
    reslult += i
  i += 1

print('1~100偶数之和是%d'%reslult)
```

测试

```
1~100偶数之和是2550
```