---
tags: [objc]
---

# 属性

## 实例变量

**Card.h**

```objc
typedef enum : NSUInteger {
	SuitHeart, // ♥️
	SuitSpade, // ♠️
	SuitDiamond, // ♦️
	SuitClub // ♣️
} Suit;

typedef enum : NSUInteger {
	ACE = 1, TWO, THREE, FOUR, FIVE, SIX,SEVEN,EIGHT, NINE, TEM, JACK, QUEEN, KING
} Rank;

@interface Card : NSObject
{
	Suit _suit;
	Rank _rank;
}

- (instancetype)initWithSuit:(Suit)suit rank:(Rank)rank;

@end
```

**Card.m**

```objc
@implementation Card

- (instancetype)initWithSuit:(Suit)suit rank:(Rank)rank {
	if (self = [super init]) {
		_suit = suit;
		_rank = rank;
	}
	return self;
}

@end
```

`_suit`、`_rank`为对象的实例变量。

## set方法 get方法 — 访问实例变量

### set和get方法

通常通过`set`和`get`方法供外部访问、设置实例变量。

**Card.h**

```objc
- (void)setSuit:(Suit)suit;
- (Suit)suit;
```

**Card.m**

```objc
- (void)setSuit:(Suit)suit {
	self->_suit = suit;
}

- (Suit)suit {
	return _suit;
}
```

### setter和getter方法命名规则

`setter`方法命名规则：**set**加属性名，并采用驼峰法命名。`getter`方法命名规则：以属性名作为getter方法名。

## `@property`属性

### @property

```objc
@property (nonatomic, assign) Suit suit;
@property (nonatomic, assign) Rank rank;
```

通过`@property`设置属性，编译器会自动添加实例变量（变量名为"_"加属性名），按照[命名规则](###set和get方法命名规则)添加`setter`和`getter`方法。同时你也可以为`setter`和`getter`方法指定一些规则。

### nonatomic/atomic

nonatomic/atomic是线程安全配置。

- **atomic**: 默认值，属性是线程安全的，速度慢。
- **nonatomic**: 不保证线程安全，速度快。

```objc
@property (atomic, copy) NSString *name;
@property (nonatomic, assign) NSInteger age;
```

### assign/strong/copy

assign/strong/copy是[[objc_base_005070内存管理|内存管理]]相关配置。

- **assign**: 基本数据类型，需要指定为assign。
- **strong**: 赋值属性强引用所赋的值。

```objc
// Demo简单说明strong作用，并非OC源码
// MRC模式
// Person自定义的一个类
- (void)setPerson:(Person *)person {
	if (_person != person) {
		[_person release];
		_person = [person retain];
	}
}
```

- **copy**: 赋值属性copy所赋的值，属性的类需要实现**NSCopying**协议。

```objc
// Demo简单说明copy作用，并非OC源码
// MRC模式
// Person自定义的一个类
- (void)setPerson:(Person *)person {
	if (_person != person) {
		[_person release];
		_person = [person copy];
	}
}
```

### readonly

`readonly`只添加`get`方法，使外部不能修改属性的值。

```objc
@interface Person : NSObject
	// 身份证
	@property (nonatomic, copy, readonly) NSString *identifier;
@end
```

### setter/getter

自定义setter/getter方法的方法名。

```objc
// 通常我们为BOOL类型属性的getter方法添加is前缀
@property (nonatomic, assign, getter=isEnabled) BOOL enabled;
```

## 重写setter和getter方法

重写`setter`和`getter`方法，可以帮助我们在设置和获取属性前后，做一些逻辑处理。比如参数检查或通知其它对象刷新等。

```objc
@interface Person : NSObject
@property (nonatomic, assign) NSInteger age;
@end

@implementation Person

- (void)setAge:(NSInteger)age {
	if (age <= 0) {
		age = 0;
	}
	_age = age;
}

@end
```

