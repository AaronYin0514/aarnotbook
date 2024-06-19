---
tags: [objc]
---

# 类

## 定义类

Objective-C类包含两部分：定义（interface）与实现（implementation）。定义部分包含类声明、实例变量的定义和类相关的方法。实现包含类的方法的实际代码。

### 声明类

声明类以`@interface`作为开头，`@end`作为结尾。

![](assets/imgs/ios/objc_class_interface.png)

### 实现类

实现类以`@implementation`作为开头，`@end`作为结尾。

![](assets/imgs/ios/objc_class_imp.png)

### 定义一个扑克牌类

**Card.h**

```objc   
typedef enum : NSUInteger {
	SuitHeart, // ♥️
	SuitSpade, // ♠️
	SuitDiamond, // ♦️
	SuitClub // ♣️
} Suit;

typedef enum : NSUInteger {
	ACE = 1,
	TWO,
	THREE,
	FOUR,
	FIVE,
	SIX,
	SEVEN,
	EIGHT,
	NINE,
	TEM,
	JACK,
	QUEEN,
	KING
} Rank;

@interface Card : NSObject

@property (nonatomic, assign) Suit suit;
@property (nonatomic, assign) Rank rank;

@end
```

**Card.m**

```objc
[[import]] "Card.h"

@implementation Card

@end
```

## 创建对象

`alloc`方法创建对象，`init`方法初始化对象。

```objc
// 创建♠️A对象
Card *card1 = [[Card alloc] init];
card1.suit = SuitSpade;
card1.rank = ACE;
```

`new`方法是`alloc` `init` 的简写方式。

```objc
// 创建♥️K对象
Card *card2 = [Card new];
card2.suit = SuitSpade;
card2.rank = KING;
```

## `init`方法

通常定义`init`开头的方法为初始化方法，初始化方法的作用是设置属性的初始值。通常我们还会添加一个工厂方法，作为创建对象的方法。

**Card.h**

```objc   
typedef enum : NSUInteger {
	SuitHeart, // ♥️
	SuitSpade, // ♠️
	SuitDiamond, // ♦️
	SuitClub // ♣️
} Suit;

typedef enum : NSUInteger {
	ACE = 1,
	TWO,
	THREE,
	FOUR,
	FIVE,
	SIX,
	SEVEN,
	EIGHT,
	NINE,
	TEM,
	JACK,
	QUEEN,
	KING
} Rank;

@interface Card : NSObject

@property (nonatomic, assign) Suit suit;
@property (nonatomic, assign) Rank rank;

- (instancetype)initWithSuit:(Suit)suit rank:(Rank)rank;
+ (instancetype)cardWithSuit:(Suit)suit rank:(Rank)rank;

@end
```

**Card.m**

```objc
[[import]] "Card.h"

@implementation Card

- (instancetype)initWithSuit:(Suit)suit rank:(Rank)rank {
	if (self = [super init]) {
		self.suit = suit;
		self.rank = rank;
	}
	return self;
}

+ (instancetype)cardWithSuit:(Suit)suit rank:(Rank)rank {
	return [[Card alloc] initWithSuit:suit rank:rank];
}

@end
```

## `description`方法

`description`是一个特殊方法，格式化输出一个对象时，会调用`description`方法并打印结果。

```objc
- (NSString *)description {
	NSString *s = @"";
	switch (self.suit) {
		case SuitHeart:
			s = @"♥️";
			break;
		case SuitSpade:
			s = @"♠️";
			break;
		case SuitDiamond:
			s = @"♦️";
			break;
		case SuitClub:
			s = @"♣️";
			break;
		default:
			break;
	}
	NSString *r = @"";
	switch (self.rank) {
		case ACE:
			r = @"A";
			break;
		case JACK:
			r = @"J";
			break;
		case QUEEN:
			r = @"Q";
			break;
		case KING:
			r = @"K";
			break;
		default:
			r = [NSString stringWithFormat:@"%ld", self.rank];
			break;
	}
	return [NSString stringWithFormat:@"%@%@", s, r];
}
```

打印对象：

```objc
// 创建♥️K对象
Card *card = [[Card alloc] initWithSuit:SuitSpade rank:KING];
NSLog(@"%@", card); // ♠️K
```

## `dealloc`方法

`dealloc`方法是对象销毁前调用的方法。该方法作用是对象销毁前[[objc_base_008000通知|移除通知]]、[[objc_base_009010定时器|移除定时器]]和[[objc_base_005050KVO|移除属性监听]]等等。

```objc
- (void)dealloc {
	NSLog(@"Card对象销毁了");
}
```