## 张量-展平-flatten

`torch.flatten` 是 PyTorch 中用于将张量展平成一维的函数。它可以按指定的起始和结束维度将多维张量展平为一个单一的向量。

### 函数签名

```python
torch.flatten(input, start_dim=0, end_dim=-1) -> Tensor
```

**参数**

- `input`: 需要展平的张量。
- `start_dim`: 开始展平的维度，默认为 `0`。
- `end_dim`: 结束展平的维度，默认为 `-1`（即最后一个维度）。

### 示例

1. **基本用法**

```python
import torch

# 创建一个3x2的张量
x = torch.tensor([[1, 2], [3, 4], [5, 6]])
print("Original tensor:")
print(x)
# tensor([[1, 2],
#         [3, 4],
#         [5, 6]])

# 展平成一维张量
x_flattened = torch.flatten(x)
print("Flattened tensor:")
print(x_flattened)
# tensor([1, 2, 3, 4, 5, 6])
```

2. **指定起始和结束维度**

```python
# 创建一个2x3x4的张量
y = torch.randn(2, 3, 4)
print("Original tensor shape:", y.shape)
# Original tensor shape: torch.Size([2, 3, 4])

# 从第2个维度开始展平
y_flattened = torch.flatten(y, start_dim=1)
print("Flattened tensor shape:", y_flattened.shape)
# Flattened tensor shape: torch.Size([2, 12])
```

3. **展平部分维度**

```python
# 创建一个2x3x4的张量
z = torch.randn(2, 3, 4)
print("Original tensor shape:", z.shape)
# Original tensor shape: torch.Size([2, 3, 4])

# 从第1个维度到第2个维度展平
z_flattened = torch.flatten(z, start_dim=1, end_dim=2)
print("Flattened tensor shape:", z_flattened.shape)
# Flattened tensor shape: torch.Size([2, 12])
```

4. **展平多维图像张量**

```python
# 创建一个批量包含2张3通道128x128的图像
images = torch.randn(2, 3, 128, 128)
print("Original tensor shape:", images.shape)
# Original tensor shape: torch.Size([2, 3, 128, 128])

# 展平除了批量维度之外的所有维度
images_flattened = torch.flatten(images, start_dim=1)
print("Flattened tensor shape:", images_flattened.shape)
# Flattened tensor shape: torch.Size([2, 49152])
```

通过这些示例，你可以看到 `torch.flatten` 如何将多维张量展平为一维张量或部分展平指定维度。这在需要将张量输入到全连接层或其他需要一维输入的情况下非常有用。

