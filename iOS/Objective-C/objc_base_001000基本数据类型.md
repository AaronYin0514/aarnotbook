---
tags: [objc]
---

# 基本数据类型


## 整数

```objc
NSInteger count = 10;
```

## 浮点数

```objc
float pi = 3.14;
double pi = 3.141592653; // double精度更高
```

## 布尔类型

```objc
BOOL isYes = YES;
BOOL isNo = NO;
```

## 字符

```objc
char x = 'a';
```

## 字符串

```objc
NSString *str = @"Hello,ObjC!";
```

## 枚举

### 定义枚举

枚举的值实质上是一个整数，默认第一个为0，并依次加1。也可以为某一项指定一个值，它后面的值默认依次加1。

```objc
typedef NS_ENUM(NSInteger, Suit) {
	SuitSpade, // 0
	SuitHeart, // 1
	SuitClub, // 2
	SuitDiamond // 3
};

typedef enum : NSUInteger {
	LanguageObjc,
	LanguageSwift,
	LanguageJava = 10, // 10
	LanguageJavascript, // 11
	LanguagePython = 20,
	LanguageRuby
} Language;
```

### 枚举的高级用法

定义一个编程语言的枚举：

```objc
typedef enum : NSUInteger {
	LanguageObjc = 1 << 0,
	LanguageSwift = 1 << 1,
	LanguageJava = 1 << 2,
	LanguageHtml = 1 << 3,
	LanguageCss = 1 << 4,
	LanguageJavascript = 1 << 5,
	LanguageWeb = LanguageHtml | LanguageCss | LanguageJavascript,
	LanguageAll = ~0UL
} Language;
```

> 解释：
> 
> 00001000 // LanguageHtml
> 
> 00010000 // LanguageCss
> 
> 00100000 // LanguageJavascript
> 
> 00111000 // LanguageWeb = LanguageHtml | LanguageCss | LanguageJavascript
> 
> 00001000 // LanguageWeb & LanguageHtml
> 
> 00000000 // LanguageWeb & LanguageObjc
> 
> 利用这个特性，我们可以完成多特性组合的功能。

表示你已经掌握OC、Swift和前端开发语言：

```objc
Language ls = LanguageObjc | LanguageSwift | LanguageWeb;
```

判断你是否掌握了OC：

```objc
if (ls & LanguageObjc) {
 	NSLog(@"你掌握了OC");
 }
```

表示你把OC给忘记了：

```objc
ls ^= LanguageObjc;
```


## 结构体

### 定义结构体

```objc
typedef struct _Person {
	NSString *name;
	NSUInteger age;
	CGFloat height;
} Person;

Person p = {@"张三", 18, 180};
```

### NSRange

NSRange通常用来表示自字符串范围。第一个参数表示索引位置，从0开始；第二个参数表示长度。

```objc
// 创建
NSRange range = NSMakeRange(2, 4); // 2表示索引，4表示长度
 NSLog(@"%@", NSStringFromRange(range));
```

### CGSize

CGSize用来表示宽度和高度，通常表示一个UI组件的大小。

```objc
CGSize size = CGSizeMake(150, 200); // 代表屏幕的宽度和高度
 NSLog(@"%@", NSStringFromSize(size));
```

### CGPoint

CGPoint用来表示坐标

```objc
CGPoint point = CGPointMake(20, 40); // 代表坐标x：20， y：40
 NSLog(@"%@", NSStringFromPoint(point));
```

### CGRect

CGReact通常表示一个UI组件的位置和大小。

- 起始点位置，即X轴和Y轴的坐标；
- 组件的大小，即组件的宽度和高度。

```objc
CGRect rect = CGRectMake(20, 30, 40, 50); // 20 30表示位置（x, y）；40 50 表示大小（宽度 高度）
NSLog(@"%@", NSStringFromRect(rect));
```

## 数组

```objc
NSArray *languages = @[
	 @"ObjC",
	 @"Swift",
	 @"JavaScript"
 ];
```

## 字典

```objc
NSDictionary *libai = @{
	@"name": @"李白",
	@"birthday": @"701"
};
```

## nil

`nil`表示空对象。

## 类型转换

```objc
CGFloat pi = 3.14;
 NSInteger pi_i = (NSInteger)pi;
```