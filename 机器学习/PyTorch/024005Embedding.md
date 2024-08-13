---
tags:
  - pytorch
  - embed
---
# Embedding

`nn.Embedding` 是 PyTorch 中用于查找表操作的一种层（layer）。它通常用于自然语言处理（NLP）中的词嵌入（word embeddings），将离散的词（或其他符号）映射到稠密的向量空间中。

## `nn.Embedding` 的工作原理

`nn.Embedding` 本质上是一个查找表。给定一个词汇表中的索引序列，它会返回相应的嵌入向量。

## 函数签名

```python
class torch.nn.Embedding(
    num_embeddings: int,
    embedding_dim: int,
    padding_idx: Optional[int] = None,
    max_norm: Optional[float] = None,
    norm_type: float = 2.0,
    scale_grad_by_freq: bool = False,
    sparse: bool = False,
    _weight: Optional[torch.Tensor] = None
)
```

### 参数说明：

‼️关键参数：

- **`num_embeddings`**: `int`，词汇表的大小，即嵌入矩阵的行数。这通常是输入数据中索引的最大值加一。例如，如果你的词汇表中有 10,000 个词，那么 `num_embeddings` 通常设置为 10,000。
- **`embedding_dim`**: `int`，每个嵌入向量的维度，决定了输出向量的长度。例如，如果你希望每个词映射到 300 维的向量空间，则 `embedding_dim` 设置为 300。

可选参数：

- **`padding_idx`**: `Optional[int]`，可选参数，指定输入中的某个索引（通常是填充标记的索引），它的嵌入向量会初始化为全零，并且在训练过程中保持不变。
- **`max_norm`**: `Optional[float]`，可选参数，如果指定了这个值，嵌入向量的范数将被裁剪到 `max_norm` 之下。
- **`norm_type`**: `float`，用于在 `max_norm` 指定时，计算向量范数的类型，默认为 `2`，即欧几里得范数。
- **`scale_grad_by_freq`**: `bool`，默认为 `False`。如果设为 `True`，会根据单词在迷你批中的频率来缩放梯度。这在处理类不平衡数据时可能有用。
- **`sparse`**: `bool`，默认为 `False`。如果设为 `True`，则会使用稀疏梯度，这对非常大的嵌入字典可能是有益的，因为稀疏更新可以节省内存。
- **`_weight`**: `Optional[torch.Tensor]`，可选参数，指定初始权重。如果指定了 `_weight`，`num_embeddings` 和 `embedding_dim` 的值会被忽略，直接使用传入的 `_weight` 作为权重矩阵。

### 返回值：

- 返回一个 `torch.nn.Embedding` 模块对象，用于将输入的整数索引映射到嵌入向量。

### 示例：

```python
import torch
import torch.nn as nn

# 创建一个Embedding层
embedding = nn.Embedding(num_embeddings=10, embedding_dim=3)

# 输入一个索引序列
input_indices = torch.tensor([1, 2, 3, 4])

# 获取对应的嵌入向量
output = embedding(input_indices)

print(output)
```

**输出**（取决于随机初始化的权重）：

```
tensor([[-0.3270, -0.3005, -0.1817],
        [ 0.1140, -0.5895,  0.1690],
        [-0.2668,  0.0393, -0.4674],
        [ 0.5925, -0.1569, -0.0128]], grad_fn=<EmbeddingBackward>)
```

### 应用场景

- **词嵌入**: 在 NLP 任务中，将单词索引映射到嵌入向量，用于表示单词的语义信息。
- **分类任务中的类嵌入**: 在多类别分类问题中，可以将类别索引映射到嵌入向量空间，帮助模型学习类别之间的关系。

