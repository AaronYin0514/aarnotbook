---
tags: [objc]
---

# 可变字符串NSMutableString

## 定义可变字符串

### 创建空可变字符串

```objc
NSMutableString *str1 = [[NSMutableString alloc] init];
```

`setString`设置字符串的值：

```objc
[str1 setString:@"Hello,ObjC!"];
```

### 初始化并指定容量

如果已知字符串的最大长度，那么可以优化可变字符串性能。

```objc
NSMutableString *str1 = [[NSMutableString alloc] initWithCapacity:10];
NSMutableString *str2 = [NSMutableString stringWithCapacity:10];
```

### 从字符串初始化

```objc
NSString *str1 = @"Hello,ObjC!";
NSMutableString *str2 = [[NSMutableString alloc] initWithString:str1];
NSMutableString *str3 = [NSMutableString stringWithString:str2];
```

## 字符串拼接

### `appendString`

```objc
NSMutableString *str1 = [NSMutableString stringWithString:@"Hello"];
[str1 appendString:@",ObjC!"];
NSLog(@"%@", str1);
```

### `appendFormat`

```objc
NSMutableString *str1 = [NSMutableString stringWithString:@"Hello"];
[str1 appendFormat:@",%@!", @"ObjC"];
NSLog(@"%@", str1);
```

## `insertString`插入字符串

```objc
NSMutableString *str1 = [NSMutableString stringWithString:@"HelloObjC!"];
[str1 insertString:@"," atIndex:5];
NSLog(@"%@", str1);
```

## 删除字符串

```objc
NSMutableString *str1 = [NSMutableString stringWithString:@"Hello,,,ObjC!"];
[str1 deleteCharactersInRange:NSMakeRange(5, 2)];
NSLog(@"%@", str1);
```

## 替换字符串

`replaceCharactersInRange:withString:`替换字符串

```objc
NSMutableString *str = [NSMutableString stringWithString:@"Hello,Java!"];
[str replaceCharactersInRange:NSMakeRange(6, 4) withString:@"ObjC"]; // Hello,ObjC!
```

`replaceOccurrencesOfString:withString:options:range:`在某范围内替换指定字符串

```objc
NSString *poetry = @"静夜思\n床前明月光，\n疑是地上霜。\n举头望明月，\n低头思故乡。";
NSMutableString *str = [NSMutableString stringWithString:poetry];
[str replaceOccurrencesOfString:@"月" withString:@"Moon" options:0 range:NSMakeRange(0, str.length)]; // 静夜思\n床前明月Moon，\n疑是地上霜。\n举头望明Moon，\n低头思故乡。
```



