---
tags: ruby
---

# 加载文件

## require

```ruby
require(name) → true or false
```

require加载指定的文件，如果加载成功则返回true，如果已经加载过则返回 false。

如果文件名解析出来不是一个绝对路径，它将会在`$LOAD_PATH`(即 `$:`) 列出的目录中被查找。

1. 如果文件名中含有扩展名`.rb`，那么它将会作为一个源文件被加载; 
2. 如果扩展名是`.so`，`.o`，或者`.dll`，或者在当前平台上默认共享的扩展名，那么Ruby把共享库加载为一个Ruby扩展。 
3. 否则，Ruby试图给传入的文件名加上`.rb`，`.so`等等直到该文件被找到。 
4. 如果指定名字的文件没能够找到，Ruby会抛出一个`LoadError`异常。

对于Ruby扩展，给定的文件名可能使用任意共享库扩展名。例如，在Linux上socket扩展名是`socket.so`， 而`require 'socket.dll'`将会加载socket扩展。

被加载文件的绝对路径会被添加到`$LOADED_FEATURES`（即`$"`）中。如果一个文件的路径已经出现在`$"`中，那么它不会被再次加载。例如，`require 'a'; require './a'`不会重复加载`a.rb`

```ruby
require "my-library.rb"
require "db-driver"
require 'csv'
```

在被加载的源文件中的任意常量或全局变量，都将在调用程序的全局空间中可用。 
但是，局部变量不会传播到加载环境中。

## require_relative

```ruby
require_relative(string) → true or false
```

以相对于当前文件路径的路径加载其他库的文件。如果文件路径无法确定，那么将会抛出一个`LoadError`异常。如果一个文件被成功加载了那么返回`true`, 否则返回`false`。

例如，你定义了两个类`Book`和`Color`，在`Book`中需要引用`Color`的相关方法，这两个文件的路径如下：

```
/home/your_name/my_ruby_files/book.rb 
/home/your_name/my_ruby_files/color.rb
```

则在book.rb中可以有如下代码：

```ruby
require_relative 'color'

class Book
  # some methods and anything else...
end
```

## 相关资料

[Ruby中require, require_relative,load,include的用法和区别](http://events.jianshu.io/p/9f2d5a0277b0)


