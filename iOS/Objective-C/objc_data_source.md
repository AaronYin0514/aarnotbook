### Card类

Card.h

```objc
typedef enum : NSUInteger {
	SuitHeart, // ♥️
	SuitSpade, // ♠️
	SuitDiamond, // ♦️
	SuitClub // ♣️
} Suit;

typedef enum : NSUInteger {
ACE = 1,TWO,THREE,FOUR,FIVE,SIX,SEVEN,EIGHT,NINE,TEM,JACK,QUEEN,KING
} Rank;

@interface Card : NSObject

@property (nonatomic, assign, readonly) Suit suit;
@property (nonatomic, assign, readonly) Rank rank;

- (instancetype)init NS_UNAVAILABLE;
- (instancetype)initWithSuit:(Suit)suit rank:(Rank)rank;
+ (instancetype)cardWithSuit:(Suit)suit rank:(Rank)rank;

@end
```

Card.m

```objc
[[import]] "Card.h"

@interface Card ()

@property (nonatomic, assign, readwrite) Suit suit;
@property (nonatomic, assign, readwrite) Rank rank;

@end

@implementation Card

- (instancetype)initWithSuit:(Suit)suit rank:(Rank)rank {
	if (self = [super init]) {
		self.suit = suit;
		self.rank = rank;
	}
	return self;
}

- (void)dealloc {
	NSLog(@"Card对象销毁了");
}

+ (instancetype)cardWithSuit:(Suit)suit rank:(Rank)rank {
	return [[Card alloc] initWithSuit:suit rank:rank];
}

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

@end
```

### Pocker

Poker.h

```objc
@interface Poker : NSObject

@property (nonatomic, copy, readonly) NSArray<Card *> *cards;

@end
```

Poker.m

```objc
@interface Poker ()

@property (nonatomic, copy, readwrite) NSArray<Card *> *cards;

@end

@implementation Poker

- (instancetype)init {
	 if (self = [super init]) {
		NSMutableArray *cards = [NSMutableArray arrayWithCapacity:52];
		for (NSInteger i = 1; i <= 13; i++) {
			for (NSInteger j = 0; j < 4; j++) {
				Card *card = [Card cardWithSuit:j rank:i];
				[cards addObject:card];
			}
		}
		self.cards = cards;
	}
	return self;
}

@end
```

定义一个洗牌方法

```objc
- (void)shuffle {
	NSMutableArray *cards = [self.cards mutableCopy];
	for (NSInteger i = 0; i < cards.count - 1; i++) {
		NSInteger j = arc4random() % (cards.count - i);
		[cards exchangeObjectAtIndex:i withObjectAtIndex:i + j];
	}
	self.cards = cards;
}
```