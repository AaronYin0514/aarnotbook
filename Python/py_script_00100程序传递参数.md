---
tags: [python, argv]
---
# 程序传递参数

## `sys.argv`

在程序中接收执行python脚本时传入的参数：

```python
import sys

print(sys.argv)
```

执行脚本：

```shell
pytho3 rest.py 1 2 a b
['rest.py', '1', '2', 'a', 'b']
```

可以看出`sys.argv`第一个参数是“rest.py”（脚本名称），后面参数依次是传入的参数。

## argparse

```python
import argparse

parser = argparse.ArgumentParser(description='manual to this script')
#parser.add_argument('-n', '--name', type=str, required=True, help="工作文件夹标号，例如242")
parser.add_argument('--name', type=str, default='张三')
parser.add_argument('--age', type=int, default=30)
parser.add_argument('--sex', type=bool, default=True)

print(parser.parse_args())
```

执行脚本：

```shell
python3 rest.py --name=Jim --age=10
Namespace(name='Jim', age=10, sex=True)

python3 rest.py --age=100 --sex=False
Namespace(name='张三', age=100, sex=True)

python3 rest.py --age=100 --sex=
Namespace(name='张三', age=100, sex=False)
```

一般使用bool、int、str、float基本类型作为参数。

### bool类型参数

> ⚠️⚠️⚠️
> 
> 其中`bool`比较特殊，**传入任何值都被解析为`True`**，**传入空时才被解析为`False`**。

### 数组类型参数

使用`nargs`参数：`nargs='+'`

```python
import argparse

parser = argparse.ArgumentParser(description='合并wav文件')
parser.add_argument('-a', '--array', type=str, required=True, nargs='+', help="要合并的音频名字")

args = parser.parse_args()
print(args) # Namespace(array=['a', 'b', 'c'])
```
