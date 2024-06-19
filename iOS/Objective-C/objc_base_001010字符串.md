---
tags: [objc]
---

# 字符串NSString

## 创建

### 字面量

通过**字面量**创建字符串：

```objc
NSString *name = @"李白";
```

### 格式化

格式化创建字符串：

```objc
NSString *name = @"李白";
 NSInteger year = 701;

 NSString *introduction = [NSString stringWithFormat:@"我的名字是%@，我生于%ld年", name, year];
 NSLog(@"%@", introduction);
```

常用格式符说明：

| 格式符号 | 说明 |
| ---- | ---- |
| %@ | 	Objective-C对象。如果该对象实现了`descriptionWithLocale`方法，则返回该方法的值；否则返回`description`方法的值。 |
| %d, %D, %i | 有符号32位整数 |
| %u, %U | 无符号32位整数 |
| %f | 64 位浮点数 (double) |
| %e, %E | 64 位浮点数 (double)，以科学记数法打印，使用小写 e, E 来引入指数 |
| %g, %G | 64 位浮点数 (double)，如果指数小于 –4 或大于或等于精度，则以 %e, %E 的样式打印，否则以 %f 的样式打印 |
| %p | 指针 |

### 多行字符串

```objc
// 使用'\'
// ⚠️注意每行缩进和空白字符串
 NSString *poetry1 = @"静夜思\
 床前明月光，\
 疑是地上霜。\
 举头望明月，\
 低头思故乡。";
 NSLog(@"%@", poetry1); // 静夜思 床前明月光， 疑是地上霜。 举头望明月， 低头思故乡。

 // 使用""
 NSString *poetry2 = @"静夜思"
 "床前明月光，"
 "疑是地上霜。"
 "举头望明月，"
 "低头思故乡。";
 NSLog(@"%@", poetry2); // 静夜思床前明月光，疑是地上霜。举头望明月，低头思故乡。

 // 使用@""
 NSString *poetry3 = @"静夜思"
 @"床前明月光，"
 @"疑是地上霜。"
 @"举头望明月，"
 @"低头思故乡。";
 NSLog(@"%@", poetry3); // 静夜思床前明月光，疑是地上霜。举头望明月，低头思故乡。
```


## `length`字符串长度

```objc
NSString *poetry = @"静夜思"
 @"床前明月光，"
 @"疑是地上霜。"
 @"举头望明月，"
 @"低头思故乡。";

 NSLog(@"%ld", poetry.length); // 27
```

## 字符串比较

### `isEqualToString`比较字符串内容

`isEqualToString`比较字符串内容是否相等：

```objc
NSString *str1 = @"Hello,ObjC!";
NSString *str2 = @"Hello,ObjC!";
if ([str1 isEqualToString:str2]) {
	NSLog(@"两个字符串内容相同");
}
```

> 使用`==`比较字符串，比较的是两个变量是否是同一个对象

### Tagged Pointer

```objc
NSString *str1 = @"Hello,ObjC!";
NSString *str2 = @"Hello,ObjC!";

if ([str1 isEqualToString:str2]) {
	NSLog(@"两个字符串内容相同");
}
if (str1 == str2) {
	NSLog(@"两个字符串是同一个对象");
}

NSString *str3 = [NSString stringWithFormat:@"%@", @"Hello,ObjC!"];
NSString *str4 = [NSString stringWithFormat:@"%@", @"Hello,ObjC!"];

if ([str3 isEqualToString:str4]) {
	NSLog(@"两个字符串内容相同");
}
if (str3 == str4) {
	NSLog(@"两个字符串是同一个对象");
}
```

## OC字符串与C字符串互转

### OC => C

```objc
NSString *ocStr = @"Hello,ObjC!";
const char *p = [ocStr UTF8String];
NSLog(@"%s", p);
```

### C => OC

```objc
const char *p = "Hello,ObjC!";
NSString *str1 = [[NSString alloc] initWithUTF8String:p];
NSString *str2 = [NSString stringWithUTF8String:p];
```

## 前缀/后缀

### `hasPrefix`是否包含前缀

```objc
NSString *poet = @"李白";
 NSLog(@"%d", [poet hasPrefix:@"李"]); // 1
```

### `hasSuffix`是否包含后缀

```objc
NSString *poet = @"李白";
NSLog(@"%d", [poet hasSuffix:@"李"]); // 0
```

## 字符串转大小写

### 字符串转大写

```objc
NSString *str = @"Hello,objC!";
 NSLog(@"%@", [str uppercaseString]); // HELLO,OBJC!
```

### 字符串转小写

```objc
NSString *str = @"Hello,objC!";
NSLog(@"%@", [str lowercaseString]); // hello,objc!
```

### 字符串转首字母大写

```objc
NSString *str = @"Hello,objC!";
NSLog(@"%@", [str capitalizedString]); // Hello,Objc!
```

## 分割

`componentsSeparatedByString`将字符串分割成数组，参数为分割符。

```objc
NSString *poetry = @"静夜思\n床前明月光，\n疑是地上霜。\n举头望明月，\n低头思故乡。";
NSArray *arr = [poetry componentsSeparatedByString:@"\n"];
 NSLog(@"%@", arr);
```

## 查找

`rangeOfString`查找字符串

```objc
NSString *poetry = @"静夜思\n床前明月光，\n疑是地上霜。\n举头望明月，\n低头思故乡。";
NSRange range = [poetry rangeOfString:@"光"];

if (range.location != NSNotFound) {
	NSLog(@"找到了 %@", NSStringFromRange(range));
} else {
	NSLog(@"没到了");
}
```

`rangeOfString:options:`按选项规则查找：

```objc
// 倒序查找
NSString *poetry = @"静夜思\n床前明月光，\n疑是地上霜。\n举头望明月，\n低头思故乡。";
NSRange r1 = [poetry rangeOfString:@"月"]; // {7, 1}
NSRange r2 = [poetry rangeOfString:@"月" options:NSBackwardsSearch]; // {22, 1}
```

## 子字符串

### `substringWithRange`

```
NSString *poetry = @"静夜思\n李白\n床前明月光，\n疑是地上霜。\n举头望明月，\n低头思故乡。";

NSRange range = NSMakeRange(4, 2);
NSString *poet = [poetry substringWithRange:range];
NSLog(@"%@", poet); // 李白
```

### `substringFromIndex`

```objc
NSString *poetry = @"静夜思\n李白\n床前明月光，\n疑是地上霜。\n举头望明月，\n低头思故乡。";

NSString *str1 = [poetry substringFromIndex:21];
NSLog(@"%@", str1); // 举头望明月，\n低头思故乡。
```

### `substringToIndex`

```objc
NSString *poetry = @"静夜思\n李白\n床前明月光，\n疑是地上霜。\n举头望明月，\n低头思故乡。";

NSString *str2 = [poetry substringToIndex:3];
NSLog(@"%@", str2); // 静夜思
```

## 拼接

### `stringWithFormat`

```objc
NSString *str1 = @"Hello";
 NSString *str2 = @"ObjC";
 NSString *h1 = [NSString stringWithFormat:@"%@,%@!", str1, str2];
```

### `stringByAppendingString`

```objc
NSString *str1 = @"Hello,";
 NSString *str2 = @"ObjC!";
 NSString *h1 = [str1 stringByAppendingString:str2];
```

### `stringByAppendingFormat`

```objc
NSString *str1 = @"Hello";
 NSString *str2 = @"ObjC";
 NSString *h1 = [str1 stringByAppendingFormat:@",%@!", str2];
```

## 字符串转换数字类型

```objc
NSString *str = @"3.14";
NSInteger i = [str integerValue];
CGFloat f = [str floatValue];
```

## 去除前后空白字符

```objc
NSString *str1 = @" Hello,ObjC! ";
NSString *str2 = [str1 stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]];
```
