---
tags: [objc]
---

# 可变数组NSMutableArray

## 定义可变数组

### 定义一个空可变数组

```objc
NSMutableArray *arr1 = [[NSMutableArray alloc] init];
NSMutableArray *arr2 = [NSMutableArray array];
```

### 从Array创建一个可变数组

```objc
NSArray *arr1 = @[@"OC", @"Swift"];
NSMutableArray *arr2 = [[NSMutableArray alloc] initWithArray:arr1];
NSMutableArray *arr3 = [NSMutableArray arrayWithArray:arr2];
```

### 创建数组指定容量

```objc
NSMutableArray *arr1 = [[NSMutableArray alloc] initWithCapacity:10];
NSMutableArray *arr2 = [NSMutableArray arrayWithCapacity:10];
```

## 添加元素

### 通过索引添加元素

```objc
NSMutableArray *arr = [NSMutableArray arrayWithCapacity:10];
arr[0] = @"OC";
arr[1] = @"Swift";
NSLog(@"%@", arr); // (OC, Swift)
```

### `addObject:`

```objc
NSMutableArray *arr = [NSMutableArray arrayWithCapacity:10];
[arr addObject:@"OC"];
[arr addObject:@"Swift"];
 NSLog(@"%@", arr); // (OC, Swift)
```

### `addObjectsFromArray:`

```objc
NSMutableArray *arr = [NSMutableArray arrayWithCapacity:10];
[arr addObjectsFromArray:@[@"OC", @"Swift"]];
NSLog(@"%@", arr); // (OC, Swift)
```

## `insertObject:`插入元素

```objc
NSMutableArray *arr = [NSMutableArray arrayWithArray:@[@"OC"]];
[arr insertObject:@"Swift" atIndex:1]; // (OC, Swift)
```

## 修改元素

### 通过索引修改元素

```objc
NSMutableArray *arr = [NSMutableArray arrayWithArray:@[@"OC", @"Swift"]];
arr[0] = @"ObjC";
NSLog(@"%@", arr); // (ObjC, Swift)
```

### `replaceObjectAtIndex:withObject:`

```objc
NSMutableArray *arr = [NSMutableArray arrayWithArray:@[@"OC", @"Swift"]];
[arr replaceObjectAtIndex:0 withObject:@"ObjC"];
NSLog(@"%@", arr); // (ObjC, Swift)
```

## 删除元素

### `removeObject:`删除某一元素

```objc
NSMutableArray *arr = [NSMutableArray arrayWithArray:@[@"ObjC", @"English", @"Swift"]];
[arr removeObject:@"English"];
NSLog(@"%@", arr); // (ObjC, Swift)
```

### `removeObjectsInArray:`删除某些元素

```objc
NSMutableArray *arr = [NSMutableArray arrayWithArray:@[@"ObjC", @"English", @"Swift", @"Chinese"]];
[arr removeObjectsInArray:@[@"English", @"Chinese"]];
NSLog(@"%@", arr); // (ObjC, Swift)
```

### `removeObjectAtIndex:`删除某一位置的元素

```objc
NSMutableArray *arr = [NSMutableArray arrayWithArray:@[@"ObjC", @"English", @"Swift"]];
[arr removeObjectAtIndex:1];
NSLog(@"%@", arr); // (ObjC, Swift)
```

### `removeAllObjects`清空数组

```objc
 NSMutableArray *arr = [NSMutableArray arrayWithArray:@[@"ObjC", @"Swift"]];
[arr removeAllObjects];
```

## `exchangeObjectAtIndex:withObjectAtIndex:`交换元素

```objc
NSMutableArray *arr = [NSMutableArray arrayWithArray:@[@"ObjC", @"Swift"]];
[arr exchangeObjectAtIndex:0 withObjectAtIndex:1];
NSLog(@"%@", arr); // (ObjC, Swift)
```

