---
tags: [python, file]
---
# 文件操作

## 文件重命名

```python
import os
os.rename('test.txt', 'test.text')
```

## 删除文件

```python
import os
os.remove('test.text')
```

## 复制文件

```python
import shutil

# 使用copy函数拷贝文件
shutil.copy('source_file.txt', 'destination_folder')

# 使用copy2函数拷贝文件，并保留源文件的元数据
shutil.copy2('source_file.txt', 'destination_folder')
```

- `copy`函数会拷贝文件并保持原始文件的权限
- `copy2`函数还会复制文件的元数据，如时间戳、权限等

![[py_base_2002文件夹操作#拷贝文件夹]]

## 移动文件(或文件夹)

```python
import shutil

# 移动文件并重命名 
shutil.move('source_file.txt', 'destination_folder/destination_file.txt')

# 移动文件夹
shutil.move('source_folder', 'destination_folder')

```

## 判断文件是否存在

判断文件是否存在

```python
import os

file_path = 'file.txt'

if os.path.exists(file_path):
    print(f"文件'{file_path}'存在")
else:
    print(f"文件'{file_path}'不存在")
```

判断给定路径是否是文件

```python
import os

os.path.isfile()
```

判断给定路径是否是文件夹（目录）

```python
import os

os.path.isdir()
```

判断给定路径是否是符号链接（软链接）

```python
import os

os.path.islink()
```


