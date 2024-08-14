---
tags:
  - pytorch
  - embed
---
# Embedding

![](embedding001.jpg)

- 将离散类别映射到稠密向量空间(保存了固定字典和大小的简单查找表)
- 能够捕捉单词之间的语义关系
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

不常用参数：

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
from torch import nn

# 创建一个Embedding层
# 定义一个具有5个单词，维度为3的查询矩阵
embedding = nn.Embedding(5, 3)
print('----- embedding.weight -----\n', embedding.weight)

# 输入一个索引序列
output = torch.tensor([[0, 2, 0, 1], [1, 3, 4, 4]]) # shape (2, 4)
print('----- 索引序列 ---------\n', output)
# 获取对应的嵌入向量
output = embedding(output) # shape (2, 4, 3) 增加的3，是因为查询向量的维度为3
print('----- output.shape ---------\n', output.shape)
print('----- output ---------------\n', output)
```

**输出**（取决于随机初始化的权重）：

```
----- embedding.weight -----
 Parameter containing:
tensor([[ 1.4455,  1.9476, -0.5451],
        [ 0.1568,  1.6262, -0.1837],
        [ 1.6919, -0.7291, -0.5700],
        [ 0.2326, -1.0678, -0.5664],
        [-2.0841,  0.2240,  0.8624]], requires_grad=True)
----- 索引序列 ---------
 tensor([[0, 2, 0, 1],
        [1, 3, 4, 4]])
----- output.shape ---------
 torch.Size([2, 4, 3])
----- output ---------------
 tensor([[[ 1.4455,  1.9476, -0.5451],
         [ 1.6919, -0.7291, -0.5700],
         [ 1.4455,  1.9476, -0.5451],
         [ 0.1568,  1.6262, -0.1837]],

        [[ 0.1568,  1.6262, -0.1837],
         [ 0.2326, -1.0678, -0.5664],
         [-2.0841,  0.2240,  0.8624],
         [-2.0841,  0.2240,  0.8624]]], grad_fn=<EmbeddingBackward0>)
```

**解释1** 查找

![](embedding-workflow.png)

**解释2**：embedding前向计算，原理的简单解释（可解释shape大小）

1. 如果我们有5000个单词的词典，`embedding_dim`设置为128（超参数），那么就生成了一个size为`(5000, 128)`的词典矩阵
2. 如果查找其中4个词，对这4个词做one-hot编码，则`shape`为`(4, 5000)`
3. 用这个张量与原词典张量相乘，就得到查找结果，`shape`为`(4, 128)`


## One-Hot 与 Embedding

**One-Hot**:

- 适用于类别***较少***
- 类别之间***没有***潜在关系的任务

**Embedding**:

- 适合处理***大量***类别
- 类别之间***有***潜在关系的任务

### 应用场景

- **词嵌入**: 在 NLP 任务中，将单词索引映射到嵌入向量，用于表示单词的语义信息。
- **分类任务中的类嵌入**: 在多类别分类问题中，可以将类别索引映射到嵌入向量空间，帮助模型学习类别之间的关系。

