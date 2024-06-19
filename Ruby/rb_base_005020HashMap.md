---
tags: [ruby, directory]
---
# HashMap
## 创建
### 使用`=>`创建HashMap

```ruby
naruto = {"name"=> "漩涡鸣人", :age=> 18, :power=> 1000}
p naruto # {"name"=>"漩涡鸣人", :age=>18, :power=>1000}
```

### 使用JSON风格创建HashMap

但是Key必须是Symbol类型，格式上要省略`:`，或写出字符串，都会被转为Symbol类型。其它类型作为Key会报错。

```ruby
naruto = {"name": "漩涡鸣人", age: 18, power: 1000}
p naruto
```

### `Hash[]`

```ruby
emptyHash = Hash[]
naruto = Hash[name: "鸣人", age: 18, power: 1000]
```

### `Hash.new`

```ruby
naruto = Hash.new()
naruto[:name] = "鸣人"
naruto[:age] = 18
```

指定默认值

```ruby
h = Hash.new(0)
p h[:age] # 0
```

## 访问
通过**key**访问**HashMap**

```ruby
naruto = {"name": "漩涡鸣人", age: 18, power: 1000}
p naruto[:name] # "漩涡鸣人"
p naruto[:age] # 18
p naruto[:foo] # nil
```

`fech`

```ruby
naruto = {name: "鸣人", age: 18, power: 1000}
# 同通过key访问
p naruto.fetch(:name) # "鸣人"
# 指定默认值
p naruto.fetch(:foo, "默认值") # "默认值"
# 通过block指定默认值，只有key不存在时会执行block
p naruto.fetch(:bar) { |key| "默认值" } # "默认值"
```

## 添加

```ruby
naruto = {"name": "漩涡鸣人", age: 18}
naruto[:power] = 1000
p naruto # {:name=>"漩涡鸣人", :age=>18, :power=>1000}
```

## 修改

```ruby
naruto = {"name": "漩涡鸣人", age: 18, power: 1000}
naruto[:power] = 2000
p naruto # {:name=>"漩涡鸣人", :age=>18, :power=>2000}
```

## 删除

```ruby
naruto = {"name": "漩涡鸣人", age: 18, power: 1000}
naruto.delete(:power)
p naruto # {:name=>"漩涡鸣人", :age=>18}
```

## `length`字典长度 

```ruby
naruto = {"name": "漩涡鸣人", age: 18, power: 1000}
p naruto.length # 3
```

## `empty?`为空判断

```ruby
h1 = {foo: "foo", bar: "bar"}
h2 = {}

p h1.empty? # false
p h2.empty? # true
```

## `include?`和`key?`和`has_key?`判断HashMap中存在key

```ruby
naruto = {name: "鸣人", age: 18, power: 1000}

p naruto.include?(:name) # true
p naruto.include?(:foo) # false

p naruto.key?(:age) # true
p naruto.has_key?(:power) # true
```

## `value?`和`has_value?`判断HashMap中存在value？

```ruby
naruto = {name: "鸣人", age: 18, power: 1000}

p naruto.has_value?("鸣人") # true
p naruto.has_value?("漩涡鸣人") # false

p naruto.value?(18) # true
```

## `keys`获取所有的key

```ruby
naruto = {name: "鸣人", age: 18, power: 1000}
p naruto.keys # [:name, :age, :power]
```


## `values`获取所有值

```ruby
naruto = {name: "鸣人", age: 18, power: 1000}
p naruto.values # ["鸣人", 18, 1000]
```

## 遍历
### `each`和`each_pair`遍历HashMap

```ruby
naruto = {name: "鸣人", age: 18, power: 1000}

naruto.each { |key, value|
	puts "#{key}, #{value}"
}

naruto.each_pair { |key, value|
	puts "#{key}, #{value}"
}
```

### `each_key`遍历HashMap所有key

```ruby
naruto = {name: "鸣人", age: 18, power: 1000}

naruto.each_key { |key|
	puts "#{key}"
}
```

### `each_value`遍历HashMap所有value

```ruby
naruto = {name: "鸣人", age: 18, power: 1000}

naruto.each_value { |value|
	puts "#{value}"
}
```

## 合并HashMap

```ruby
merge → copy_of_self

merge(*other_hashes) → new_hash

merge(*other_hashes) { |key, old_value, new_value| ... } → new_hash
```

合并多个HashMap，新值会覆盖旧值。`merge!`会改变原值。

```ruby
h = {foo: 0, bar: 1, baz: 2}
h1 = {bat: 3, bar: 4}
h2 = {bam: 5, bat:6}
h.merge(h1, h2) # => {:foo=>0, :bar=>4, :baz=>2, :bat=>6, :bam=>5}
```
