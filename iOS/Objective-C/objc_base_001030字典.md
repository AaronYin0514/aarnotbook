---
tags: [objc]
---

# 字典NSDictionary

## 定义

### 字面量

```objc
NSDictionary *card = @{
	 @"number": @(6),
	 @"suit": @"♠️"
 };
```

## 访问

```objc
NSDictionary *card = @{
	 @"number": @(6),
	 @"suit": @"♠️"
 };

NSString *suit = card[@"suit"]; // ♠️
NSNumber *num = [card objectForKey:@"number"]; // 6
```

## 字典键值对个数

```objc
NSDictionary *card = @{@"number": @(6), @"suit": @"♠️"};
NSInteger count =card.count; // 2
```

## 获取所有的key

```objc
NSDictionary *card = @{
	@"number": @(6),
	@"suit": @"♠️"
};

NSArray *keys = card.allKeys;
```

## 获取所有值

```objc
NSDictionary *card = @{
	@"number": @(6),
	@"suit": @"♠️"
};

NSArray *keys = card.allValues;
```

## 遍历

```objc
NSDictionary *card = @{
	@"number": @(6),
	@"suit": @"♠️"
};  
[card enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop) {
	NSLog(@"key: %@; value: %@", key, obj);
}];
```



