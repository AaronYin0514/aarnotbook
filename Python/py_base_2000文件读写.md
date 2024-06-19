---
tags: [python, file]
---
# 文件读写

## open - 打开文件

```python
f = open('test.text', 'w')
```

说明

| 访问模式 | 说明 |
| ---- | ---- |
| r | 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。
| w | 打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| a | 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| rb | 以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。 |
| wb | 以二进制格式打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| ab | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| r+ | 打开一个文件用于读写。文件指针将会放在文件的开头。 |
| w+ | 打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| a+ | 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。 |
| rb+ | 以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。 |
| wb+ | 以二进制格式打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| ab+ | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。 |

## close - 关闭文件

```python
f = open('test.text', 'w')
f.close()
```

## write - 写入数据

使用`write()`可以完成向文件写入数据

```python
f = open('test.txt', 'w')
f.write('life is shot, you need python')
f.close()
```

## read - 读取数据

### read - 按字节读取 & 读取全部

使用`read(num)`可以从文件中读取数据，num表示要从文件中读取的数据的长度（单位是字节），默认读取文件中所有的数据

```python
f = open('test.txt', 'r')
content = f.read(12)
print(content) # life is shot
f.close()
```

### readline readlines - 按行读取

`readline`按行读取数据

```python
f = open('test.txt', 'w')
f.write('life is shot,\n you need python')
f.close()

f = open('test.txt', 'r')
content = f.readline()
print('第1行: ' + content) # 第1行: life is shot,
content = f.readline()
print('第2行: ' + content) # 第2行:  you need python
f.close()
```

`readlines`读取全部数据，按行返回

```python
f = open('test.txt', 'w')
f.write('life is shot,\n you need python')
f.close()

f = open('test.txt', 'r')
contents = f.readlines()
print(contents) # ['life is shot,\n', ' you need python']
f.close()
```

## tell - 获取当前读写的位置

```python
f = open('test.txt', 'r')
content = f.read(3)
position = f.tell()
print(position) # 3
content = f.read(3)
position = f.tell()
print(position) # 6
f.close()
```

## 定位到某个位置

定位接口:

```
seek(offset, from)
```

* offset: 偏移量
* from: 0 - 文件开头；1 - 当前位置；2 - 文件末尾

**设置读取位置到8**

```python
# test.txt
# life is short,\n you need python

f = open('test.txt', 'r')
f.seek(8, 0)
content = f.read(5)
print(content) # short
```

**从文件末尾定位**，需要以b模式打开文件，否则只允许从文件头开始计算相对位置

```python
# test.txt
# life is short,\n you need python

f = open('test.txt', 'rb')
f.seek(-6, 2)
content = f.read(6)
print(content) # b'python'
```



