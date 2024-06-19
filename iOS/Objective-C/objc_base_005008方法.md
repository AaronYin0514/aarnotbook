---
tags: [objc]
---

# 方法

Objective-C类的方法包括类方法和实例方法。实例方法只能有实例对象调用。类方法由类调用。

## 实例方法

### 定义实例方法

实例方法声明包括方法类型标识符(`-`)，返回值类型，一个或多个方法标识关键字，参数类型和参数名。无返回值类型为`void`。

![](assets/imgs/ios/objc_class_method.png)

为[[objc_data_source#Card类|扑克Card类]]添加两个实例方法，返回Card的花色、类型的字符串。

```objc
- (NSString *)suitStr {
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
	return s;
}

- (NSString *)rankStr {
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
	return r;
}
```

### 调用实例方法

调用方法被`[]`包括，左边是调用方法的实例对象，右边是方法。

```objc
Card *card = [[Card alloc] initWithSuit:SuitSpade rank:KING];
NSString *sult = [card suitStr];
NSLog(@"%@", sult);
```

## 类方法

### 定义类方法

类方法声明包括方法类型标识符(`+`)，返回值类型，一个或多个方法标识关键字，参数类型和参数名。无返回值类型为`void`。

![](assets/imgs/ios/objc_class_method_c.png)

定义一个比较牌点数大小的类方法：

```objc
+ (NSInteger)compareWithCard:(Card *)card1 andCard:(Card *)card2 {
	if (card1.rank > card2.rank) {
		return 1;
	} else if (card1.rank == card2.rank) {
		return 0;
	} else {
		return -1;
	}
}
```

### 调用类方法

调用方法被`[]`包括，左边是调用方法的类，右边是方法。

```objc
Card *card1 = [[Card alloc] initWithSuit:SuitSpade rank:KING];
Card *card2 = [[Card alloc] initWithSuit:SuitHeart rank:JACK];

NSInteger result = [Card compareWithCard:card1 andCard:card2];
NSLog(@"比较结果: %ld", result);
```

### 返回值

在函数体内通过`return`返回函数的结果。

```objc
// 重写description，返回排名
- (NSString *)description {
return [NSString stringWithFormat:@"%@%@", [self suitStr], [self rankStr]];
}
```

调用：

```objc
Card *card = [[Card alloc] initWithSuit:SuitSpade rank:KING];
NSLog(@"%@", sult); // ♠️K
```

## 参数

### 方法参数

定义一个方法，传入Card数组，返回这是一组什么牌。

```objc
+ (NSString *)madeUpNameWithCards:(NSArray *)cards {
	NSString *name = @"杂牌";
	if (cards.count == 2) {
		Card *card1 = cards[0];
		Card *card2 = cards[1];
		if (card1.rank == card2.rank) {
			name = [NSString stringWithFormat:@"一对%@", [card1 rankStr]];
		}
	} else if (cards.count == 3) {
		Card *card1 = cards[0];
		Card *card2 = cards[1];
		Card *card3 = cards[2];
		if (card1.rank == card2.rank && card1.rank == card3.rank) {
			name = [NSString stringWithFormat:@"三条%@", [card1 rankStr]];
		}
	}
	return name;
}
```

调用：

```objc
Card *card1 = [[Card alloc] initWithSuit:SuitSpade rank:KING];
Card *card2 = [[Card alloc] initWithSuit:SuitDiamond rank:KING];
Card *card3 = [[Card alloc] initWithSuit:SuitSpade rank:ACE];

NSString *r1 = [Card madeUpNameWithCards:@[card1, card2]];
NSLog(@"%@", r1); // 一对K

NSString *r2 = [Card madeUpNameWithCards:@[card1, card2, card3]];
NSLog(@"%@", r2); // 杂牌
```

### 不定长参数

```objc
+ (NSString *)madeUpNameWithCards:(Card *)card, ... {
	NSMutableArray *cards = [NSMutableArray array];
	va_list arg_list;
	va_start(arg_list, card);
	[cards addObject:card];
	Card *obj = va_arg(arg_list, Card *);
	while (obj) {
		[cards addObject:obj];
		obj = va_arg(arg_list, Card *);
	}
	va_end(arg_list);
	
	NSString *name = @"杂牌";
	if (cards.count == 2) {
		Card *card1 = cards[0];
		Card *card2 = cards[1];
		if (card1.rank == card2.rank) {
			name = [NSString stringWithFormat:@"一对%@", [card1 rankStr]];
		}
	} else if (cards.count == 3) {
		Card *card1 = cards[0];
		Card *card2 = cards[1];
		Card *card3 = cards[2];
		if (card1.rank == card2.rank && card1.rank == card3.rank) {
			name = [NSString stringWithFormat:@"三条%@", [card1 rankStr]];
		}
	}
	return name;
}
```

函数调用传递不定长参数时，最后要传如`nil`表示参数传入结束。

```objc
Card *card1 = [[Card alloc] initWithSuit:SuitSpade rank:KING];
Card *card2 = [[Card alloc] initWithSuit:SuitDiamond rank:KING];
Card *card3 = [[Card alloc] initWithSuit:SuitSpade rank:ACE];

NSString *r1 = [Card madeUpNameWithCards:card1, card2, nil];
NSLog(@"%@", r1); // 一对K

NSString *r2 = [Card madeUpNameWithCards:card1, card2, card3, nil];
NSLog(@"%@", r2); // 杂牌
```

