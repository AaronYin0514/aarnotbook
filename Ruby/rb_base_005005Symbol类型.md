---
tags: [ruby, symbol]
---
# Symbol

```ruby
a = "Hello,Ruby!"
b = "Hello,Ruby!"
p a.object_id == b.object_id # false

a = :"Hello,Ruby!"
b = :"Hello,Ruby!"
a.object_id == b.object_id # true
```

ruby中两个字面量相同的字符串是两个不同的变量。而Symbol保证是唯一的对象。这样节省了内存。通常Symbol类型应用在HashMap设置Key。

## 创建

```ruby
# 字面量创建 语法 :值
name = :name
```

## 字符串转Symbol

```ruby
foo = "foo".to_sym
p foo # :foo
```


## Symbol转字符串/数值

```ruby
foo = :foo.to_s
p foo # "foo"
```
