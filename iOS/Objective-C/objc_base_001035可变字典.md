---
tags: [objc]
---

# 可变字典NSMutableDictionary

## 定义

### 创建空字典

```objc
NSMutableDictionary *dict1 = [[NSMutableDictionary alloc] init];
NSMutableDictionary *dict2 = [NSMutableDictionary dictionary];
```

### 从字典创建

```objc
NSDictionary *card = @{
	 @"number": @(6),
	 @"suit": @"♠️"
};

NSMutableDictionary *dict1 = [[NSMutableDictionary alloc] initWithDictionary:card];
NSMutableDictionary *dict2 = [NSMutableDictionary dictionaryWithDictionary:card];
```

### 创建并指定容量

```objc
NSMutableDictionary *dict1 = [[NSMutableDictionary alloc] initWithCapacity:10];
NSMutableDictionary *dict2 = [NSMutableDictionary dictionaryWithCapacity:10];
```

## 添加/修改

### 添加/修改

```objc
NSMutableDictionary *card = [NSMutableDictionary dictionaryWithCapacity:2];
// dict[key]=value语法创建
card[@"number"] = @(6);
// setValue:forKey:
[card setValue:@"♥️" forKey:@"suit"];
```

### `addEntriesFromDictionary:`从字典添加

```objc  
NSMutableDictionary *person = [NSMutableDictionary dictionaryWithCapacity:10];
 person[@"name"] = @"张三";
[person addEntriesFromDictionary:@{
	@"age": @18,
	@"TEL": @"12232433564"
}];
```

## 删除

### 根据key删除某个键值对

```objc
NSMutableDictionary *person = [NSMutableDictionary dictionaryWithDictionary:@{
	@"name": @"张三",
	@"age": @18,
	@"TEL": @"12232433564"
}];
[person removeObjectForKey:@"TEL"];
```

### 批量删除键值对

```objc
NSMutableDictionary *person = [NSMutableDictionary dictionaryWithDictionary:@{
	@"name": @"张三",
	@"age": @18,
	@"TEL": @"12232433564"
}];
[person removeObjectsForKeys:@[@"TEL", @"age"]];
```
