---
tags: [ruby, closure]
---
# Proc类

## Proc类

### 定义

#### new

```ruby
hello1 = Proc.new do |name|
  puts "Hello, #{name}."
end
```

#### proc


```ruby
hello2 = proc do |name|
  puts "Hello, #{name}."
end
```


### 执行

使用`Proc#call`或`Proc#[]`执行块

```ruby
# 判断西历的年是否为闰年的处理
leap = Proc.new do |year|
  year % 4 == 0 && year % 100 != 0 || year % 400 ==0
end
　
p leap.call(2000)    #=> true
p leap[2013]         #=> false
p leap[2016]         #=> true
```


### 作为参数接收块

在变量名前添加 `&` 的参数被称为 Proc 参数。如果在调用方法时没有传递块，`Proc` 参数的值就为 `nil`。

```ruby
def total2(from, to, &block)
  result = 0               # 合计值
  from.upto(to) do |num|   # 处理从 from 到 to 的值
    if block               #   如果有块的话
      result +=            #     累加经过块处理的值
   block.call(num)
    else                   #   如果没有块的话
      result += num        #     直接累加
    end
  end
  return result            # 返回方法的结果
end

p total2(1, 10)                   # 从 1 到 10 的和 => 55
p total2(1, 10){|num| num ** 2 }  # 从 1 到 10 的 2 次冥的和 => 385
```

## lambda

Proc另外一种写法叫 `lambda`。与 `Proc.new`、`proc` 一样，`lambda` 也可以创建 `Proc` 对象，但通过 `lambda` 创建的 `Proc` 的行为会更接近方法。

第一个不同点是，`lambda` 的参数数量的检查更加严密。对用 `Proc.new` 创建的 `Proc` 对象调用 `call` 方法时，`call` 方法的参数数量与块变量的数量可以不同。但通过 `lambda` 创建 `Proc` 对象时，如果参数数量不正确，程序就会产生错误。



