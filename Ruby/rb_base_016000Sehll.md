---
tags: ruby, shell
---

# Ruby执行Shell

## 执行shell方式一

### 执行shell

```ruby
`echo 'Test'`
```

### 打印执行结果

```ruby
res = `echo 'Test'`
puts res
```

### 判读执行是否成功

```ruby
require 'English'

`echo 'Test'`

if $CHILD_STATUS.success?
	puts '执行成功'
else
	puts '执行失败'
end
```

