# numpy

## numpy的数据类型

| 类型                                  | 类型代码        | 说明                                                    |
| ------------------------------------- | --------------- | ------------------------------------------------------- |
| `int8` `uint8`                        | `i1` `u1`       | 有符号和无符号的8位（1个字节）整型                      |
| `int16` `uint16`                      | `i2` `u2`       | 有符号和无符号的16位（2个字节）整型                     |
| `int32` `uint32`                      | `i4` `u4`       | 有符号和无符号的32位 （4个字节）𤨣型                    |
| `int64` `uint64`                      | `i8` `u8`       | 有符号和无符号的64位（8个字节）𤨣型                     |
| `float16`                             | `f2`            | 半精度浮点数                                            |
| `float32`                             | `f4` `f`        | 标准的单精度浮点数。与C的float兼容                      |
| `float64`                             | `f8` `d`        | 标准的双精度浮点数。与C的double 和Python的float对象兼容 |
| `float128`                            | `f16` `g`       | 扩展精度浮点数                                          |
| `complex64` `complex128` `complex256` | `c8` `c16` `32` | 分别用两个32位、64位或128位浮点数表示的复数             |
| `bool`                                |                 | 存储`True`和`False`值的布尔类型                         |

## 数组

### 创建数组

#### 通过`List`创建

```python
import numpy as np

a1 = np.array([1,2,3])
print(a1) # [1 2 3]
```

#### 通过`range`创建数组

```python
a2 = np.array(range(5))
print(a2) # [0 1 2 3 4]
```

```python
a3 = np.arange(1, 10, 3) # 3表示步长
print(a3) # [1 4 7]
```

#### `numpy`数组的类型

```python
a1 = np.array([1,2,3])
print(type(a1)) # <class 'numpy.ndarray'>
```

#### `numpy`数组的数据类型

`dtype`属性查看数组数据类型

```python
a1 = np.array([1,2,3])
print(a1.dtype) # int64
```

创建数组指定数据类型

```python
a4 = np.arange(5, dtype=int)
print(a4.dtype) # int64

a4 = np.arange(5, dtype='i1')
print(a4.dtype) # int8

a4 = np.array([1,0,1], dtype=bool)
print(a4) # [ True False  True]
print(a4.dtype) # bool
```

修改数据类型

```python
a5 = a4.astype('f2')
print(a5) # [1. 0. 1.]
print(a5.dtype) # float16
```








