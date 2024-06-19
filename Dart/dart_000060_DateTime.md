---
tags: dart date
---

# DateTime
## 创建时间

### 创建当前时间

```dart
DateTime now = DateTime.now();
print(now); // 2022-06-25 14:08:54.021936
```

### 年[月日时分秒]创建时间

```dart
DateTime d = DateTime(2020, 6, 25, 10, 10);
print(d); // 2020-06-25 10:10:00.000
```

### 时间戳创建时间

```dart
DateTime d2 = DateTime.fromMillisecondsSinceEpoch(1656137508961);
print(d2); // 2022-06-25 14:11:48.961
```

### 字符串创建时间
```dart
DateTime d4 = DateTime.parse('2020-10-10 09:30:30');
print(d4); // 2020-10-10 09:30:30.000
```

### utc时间

```dart
DateTime d3 = DateTime.utc(2020, 6, 25, 10, 10);
print(d3); // 2020-06-25 10:10:00.000Z
```

## 时间戳

```dart
DateTime now = DateTime.now();
print(now); // 2022-06-25 14:08:54.021936

print(now.microsecondsSinceEpoch); // 1656137452757614 微秒
print(now.millisecondsSinceEpoch); // 1656137508961 毫秒
```

## 时间计算

### 时间差

```dart
DateTime d1 = DateTime(2020, 6, 20, 11, 11, 11);
DateTime d2 = DateTime(2020, 6, 20, 12, 11, 11);

var diff = d1.difference(d2);
print(diff.inHours); // -1
print(diff.inMinutes); // -60
```

### 时间增减

```dart
DateTime d1 = DateTime(2020, 6, 20, 11, 11, 11);

print(d1.add(Duration(seconds: 11))); // 2020-06-20 11:11:22.000
print(d1.add(Duration(seconds: -11))); // 2020-06-20 11:11:00.000
print(d1.subtract(Duration(seconds: 11))); // 2020-06-20 11:11:00.000
```




