---
tags: ruby, file
---

# 文件IO

## 输入/输出种类

- **标准输入**：获取键盘输入的内容。通过预定义常量 `STDIN` 调用。也可以用全局变量 `$stdin` 引用。
- **标准输出**：输出数据显示在屏幕中。通过预定义常量 `STDOUT` 调用。也可以用全局变量 `$stdout`引用。
- **标准错误输出**：向标准错误输出写入的数据会显示在屏幕中。通过预定义常量 `STDERR` 调用。也可以用全局变量 `$stderr` 引用。

## 文件File

通过 `IO` 类的子类 `File` 类可以进行文件的输入 / 输出处理。

### open打开文件

#### open

```ruby
io = File.open(file, mode)
```

mode模式

| 模式 | 意义 |
| ---- | ---- |
|  r    |  用只读模式打开文件。    |
|  r+    | 用读写模式打开文件。    |
|  w    |  用只写模式打开文件。文件不存在则创建新的文件；文件已存在则清空文件，即将文件大小设置为0。    |
|  w+    | 读写模式，其余同 `"w"` 。     |
|  a    |  用追加模式打开文件。文件不存在则创建新的文件。    |
|  a+    | 用读取/追加模式打开文件。文件不存在则创建新的文件。     |

#### 使用块

`File.open` 方法如果使用块，则文件会在使用完毕后自动关闭。

```ruby
File.open("foo.txt") do |io|
  while line = io.gets
    puts line
  end
end
```

### close关闭文件

#### close

使用`close`方法关闭文件。文件使用完毕后应尽快关闭，如果多个文件不关闭，调用`open`方法可能异常。

```ruby
io.close
```

#### close?判断文件是否关闭

```ruby
io = File.open("foo.txt")
io.close
p io.closed?    #=> true
```

### 读取文件

#### read

`read` 可以一次性读取文件 `file` 的内容。

```ruby
data = File.read("foo.txt")
```

### 输入操作

#### 按行读取

```ruby
io.gets(rs)
io.each(rs)
io.each_line(rs)
io.readlines(rs)
```


### 输出操作







