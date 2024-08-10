---
tags:
  - python
  - permute
---

# 张量-改变维度顺序-permute

`torch.permute` 是 PyTorch 中用于更改张量维度顺序的函数。它允许你指定维度的新顺序，从而返回一个新的张量，而不会改变原始张量的内容。此操作通常在需要调整张量的维度顺序以符合特定操作或模型的要求时使用。

### 函数签名

```python
torch.Tensor.permute(*dims) -> Tensor
```

### 参数

- `dims`: 一个整数序列，表示新的维度顺序。

### 示例

1. **基本用法**

```python
import torch

# 创建一个3x2x4的张量
x = torch.randn(3, 2, 4)
print("Original tensor shape:", x.shape)
# Original tensor shape: torch.Size([3, 2, 4])

# 使用 permute 改变维度顺序
y = x.permute(1, 0, 2)
print("Permuted tensor shape:", y.shape)
# Permuted tensor shape: torch.Size([2, 3, 4])
```

在上面的示例中，原始张量 `x` 的形状是 `[3, 2, 4]`。通过 `x.permute(1, 0, 2)`，我们将维度顺序从 `[0, 1, 2]` 改变为 `[1, 0, 2]`，得到的张量 `y` 的形状是 `[2, 3, 4]`。

2. **使用 permute 调整图像的维度**

在处理图像数据时，通常需要将图像从形状 `[Height, Width, Channels]` 调整为 `[Channels, Height, Width]`。

```python
# 创建一个具有形状[Height, Width, Channels]的张量
image = torch.randn(128, 128, 3)
print("Original image shape:", image.shape)
# Original image shape: torch.Size([128, 128, 3])

# 调整图像的维度顺序
image_permuted = image.permute(2, 0, 1)
print("Permuted image shape:", image_permuted.shape)
# Permuted image shape: torch.Size([3, 128, 128])
```

