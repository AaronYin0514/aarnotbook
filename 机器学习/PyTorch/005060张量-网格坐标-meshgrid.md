---
tags:
  - pytorch
  - meshgrid
---
好的，让我们重新整理一下 `torch.meshgrid` 的文档，包括函数签名、参数、返回值和示例。

---

## 张量-网格坐标-`torch.meshgrid` 

`torch.meshgrid` 用于生成多维网格坐标，适用于科学计算和图像处理等领域。

### 函数签名

```python
torch.meshgrid(*tensors, indexing: Optional[str] = 'ij') -> List[Tensor]
```

### 参数

- `*tensors`：一组 1D 张量，用于生成网格。这些张量表示每个维度的坐标。
- `indexing` (可选)：索引方式，可以是 `'xy'` 或 `'ij'`。默认值为 `'ij'`。
  - `'xy'`：适用于笛卡尔坐标系，通常用于图像处理。
  - `'ij'`：适用于多维数组的索引（矩阵索引），通常用于科学计算。

### 返回值

返回一个张量列表，这些张量代表生成的网格坐标。


### 计算过程

1. **接收输入张量**：假设输入张量为 $t_1, t_2, \dots, t_n$。
2. **生成网格坐标**：对于每个输入张量，`torch.meshgrid` 会生成相应的网格坐标，使得每个输出张量在对应维度上重复输入张量的值，而在其他维度上进行扩展。

#### 通俗理解

**使用 `'xy'` 索引方式**

```python
x = torch.tensor([1, 2, 3])
y = torch.tensor([4, 5])
z = torch.tensor([8, 9])

xx, yy, zz = torch.meshgrid(x, y, z, indexing='ij')
```

`x`在其它维度扩展，则`shape`为$2 \times 2$（`y`是2，`z`是2），`x`上重复，

* `1` -> `[[1, 1], [1, 1]]`
* `2` -> `[[2, 2], [2, 2]]`
* `3` -> `[[3, 3], [3, 3]]`

`xx`结果为

```
tensor([[[1, 1], [1, 1]], [[2, 2], [2, 2]], [[3, 3], [3, 3]]])
```

### 示例

#### 使用 `'ij'` 索引方式

```python
import torch

x = torch.tensor([1, 2, 3])
y = torch.tensor([4, 5])

xx, yy = torch.meshgrid(x, y, indexing='ij')

print("Using 'ij' indexing:")
print("xx:")
print(xx)
print("yy:")
print(yy)
```

输出：

```plaintext
Using 'ij' indexing:
xx:
tensor([[1, 1],
        [2, 2],
        [3, 3]])
yy:
tensor([[4, 5],
        [4, 5],
        [4, 5]])
```

#### 使用 `'xy'` 索引方式

```python
xx, yy = torch.meshgrid(x, y, indexing='xy')

print("Using 'xy' indexing:")
print("xx:")
print(xx)
print("yy:")
print(yy)
```

输出：

```plaintext
Using 'xy' indexing:
xx:
tensor([[1, 2, 3],
        [1, 2, 3]])
yy:
tensor([[4, 4, 4],
        [5, 5, 5]])
```

在这个例子中：

* `xx` 的每一列都是 `x` 的值$[1, 2, 3]$进行扩展。
* `yy` 的每一行都是 `y` 的值 $[4, 5]$ 进行扩展。

### 应用场景

1. **绘制函数图像**

使用 `torch.meshgrid` 生成二维坐标网格，然后计算函数值，并绘制图像。

```python
import matplotlib.pyplot as plt

x = torch.linspace(-5, 5, 100)
y = torch.linspace(-5, 5, 100)
xx, yy = torch.meshgrid(x, y, indexing='ij')
zz = torch.sin(torch.sqrt(xx**2 + yy**2))

plt.contourf(xx.numpy(), yy.numpy(), zz.numpy(), cmap='viridis')
plt.colorbar()
plt.show()
```

2. **图像处理**

在图像处理中，生成图像像素的坐标网格，然后对这些坐标进行变换。

```python
height, width = 256, 256
x = torch.linspace(0, width-1, width)
y = torch.linspace(0, height-1, height)
xx, yy = torch.meshgrid(x, y, indexing='xy')

# 将网格坐标转换为图像中的坐标
coords = torch.stack([xx, yy], dim=-1)
print(coords.shape)  # 输出: torch.Size([256, 256, 2])
```

### 总结

`torch.meshgrid` 是一个非常有用的函数，可以用于生成多维网格坐标。通过指定 `indexing` 参数，可以控制生成的网格的索引方式，以适应不同的应用场景。在使用 `torch.meshgrid` 时，显式地传递 `indexing` 参数可以确保代码在未来的 PyTorch 版本中保持兼容性。