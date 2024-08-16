---
tags:
  - pytorch
  - repeat_interleave
  - tensor
---
## 张量-复制元素-repeat_interleave

`torch.repeat_interleave` 是 PyTorch 中的一个函数，用于沿指定维度重复张量中的元素。这个函数可以根据指定的次数来复制每个元素，生成一个新的张量。

### 函数签名

```python
torch.repeat_interleave(input, repeats, dim=None, output_size=None)
```

### 参数说明

- **input**: 输入张量。
- **repeats**: 表示每个元素需要重复的次数。可以是一个整数或一个与输入张量在指定维度上长度相同的张量/列表。
- **dim**: 指定沿哪个维度重复元素。如果不指定，输入张量将被展平（flatten）并沿展平后的维度进行重复。
- **output_size**: 可选参数，用于指定输出张量的大小。如果指定，该大小将用于计算输出张量的维度。

### 返回值

返回一个新的张量，其中每个元素按照 `repeats` 指定的次数重复。

### 示例

#### 沿指定维度重复元素

```python
import torch

# 创建一个 1D 张量
x = torch.tensor([1, 2, 3])
# 沿着张量展开的维度，每个元素重复 2 次
result = torch.repeat_interleave(x, repeats=2)
print(result)  # 输出: tensor([1, 1, 2, 2, 3, 3])
```

#### 沿指定维度重复元素（2D 张量）

```python
import torch

# 创建一个 2D 张量
x = torch.tensor([[1, 2], [3, 4]])
# 沿着第 0 维（行）重复每个元素 2 次
result = torch.repeat_interleave(x, repeats=2, dim=0)
print(result)
# 输出:
# tensor([[1, 2],
#         [1, 2],
#         [3, 4],
#         [3, 4]])

# 沿着第 1 维（列）重复每个元素 2 次
result = torch.repeat_interleave(x, repeats=2, dim=1)
print(result)
# 输出:
# tensor([[1, 1, 2, 2],
#         [3, 3, 4, 4]])
```

#### 不同元素有不同的重复次数

```python
import torch

# 创建一个 1D 张量
x = torch.tensor([1, 2, 3])
# 使用一个列表来指定每个元素的重复次数
result = torch.repeat_interleave(x, repeats=[1, 2, 3])
print(result)  # 输出: tensor([1, 2, 2, 3, 3, 3])
```

### 总结

`torch.repeat_interleave` 是一个强大的函数，用于根据指定的重复次数扩展张量中的元素。它可以用于数据预处理、数据扩展以及某些需要重复数据的操作。
