---
tags: [objc]
---

# 数组NSArray

## 定义

### 字面量

```objc
NSArray *arr1 = @[
	@"ObjC",
	@"Swift",
	@"JavaScript",
];
```

### `initWithObjects`

`nil`表示结束。

```objc
NSArray *arr2 = [[NSArray alloc] initWithObjects:@"ObjC", @"Swift", @"JavaScript", nil];
```

## 访问

### 下标索引

```objc
NSArray *arr = @[@"ObjC", @"Swift", @"JavaScript"];
arr[0]; // "ObjC"
arr[2]; // "JavaScript"]
```

### `objectAtIndex:`

```objc
NSArray *arr = @[@"ObjC", @"Swift", @"JavaScript"];
[arr objectAtIndex:0]; // "ObjC"
```

### `firstObject`第一个元素

返回数组第一个元素，如果数组为空，那么返回nil。可以避免数组越界。

```objc
NSArray *arr = @[@"ObjC", @"Swift", @"JavaScript"];
[arr firstObject]; // "ObjC"
```

### `lastObject`最后一个元素

返回数组最后一个元素，如果数据为空，那么返回nil。可以避免数组越界。

```objc
NSArray *arr = @[@"ObjC", @"Swift", @"JavaScript"];
[arr lastObject]; // "JavaScript"
```

## `count`长度

```objc
NSArray *arr1 = @[@"ObjC", @"Swift", @"JavaScript"];
 arr1.count; // 3
```

### `containsObject`判断是否存在某值

```object
NSArray *arr = @[@"ObjC", @"Swift", @"JavaScript"];
if ([arr containsObject:@"Swift"]) {
	NSLog(@"包含Swift");
}
```

## 遍历数组

### for循环

```objc
NSArray *arr = @[@"ObjC", @"Swift", @"JavaScript"];
for (NSInteger i = 0; i < arr.count; i++) {
	NSLog(@"%@", arr[i]);
}
```

### for-in

```objc
NSArray *arr = @[@"ObjC", @"Swift", @"JavaScript"];
for (NSString *l in arr) {
	NSLog(@"%@", l);
}
```

### block

```objc
NSArray *arr = @[@"ObjC", @"Swift", @"JavaScript",];
[arr enumerateObjectsUsingBlock:^(id _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
	NSLog(@"索引：%ld 值：%@", idx, obj);
}];
```

在block中停止遍历：

```objc
*stop = NO;
```

## 数组拼接

### `arrayByAddingObject`拼接元素

```objc
NSArray *arr = @[@"ObjC", @"Swift"];
arr = [arr arrayByAddingObject:@"JS"]; // (ObjC, Swift, JS)
```

### `arrayByAddingObjectsFromArray`拼接数组

```objc
NSArray *arr = @[@"ObjC", @"Swift"];
arr = [arr arrayByAddingObjectsFromArray:@[@"JS", @"HTML"]]; // (ObjC, Swift, JS, HTML)
```

## 数组=>字符串

```objc
NSArray *arr = @[@"ObjC", @"Swift", @"JavaScript"];
NSString *str = [arr componentsJoinedByString:@"/"]; // ObjC/Swift/JavaScript
```
