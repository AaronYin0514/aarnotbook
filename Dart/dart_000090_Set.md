---
tags: dart set
---

# Set

## 创建

```dart
Set<String> set = {"OC", "Swift", "Dart"};
set.add("OC");
set.add("Swift");
set.add("Dart");
set.add("Dart");
print(set); // {OC, Swift, Dart}
```

## 访问

### 按条件访问`firstWhere` `lastWhere`

```dart
Set<String> set = {'Swift', 'Java', 'Dart', 'JavaScript'};

String str1 = set.firstWhere((element) => element.contains('Java'));
print(str1); // Java

String str2 = set.lastWhere((element) => element.contains('Java'));
print(str2); // JavaScript
```

### 第一个元素`first`

```dart
Set<String> set = {"OC", "Swift", "Dart"};
print(set.first); // OC
```

### 最后一个元素`last`

```dart
Set<String> set = {"OC", "Swift", "Dart"};
print(set.last); // Dart
```

### 判断存在某(个)元素`contains` `containsAll`

```dart
Set<String> set = {'Swift', 'Java', 'Dart', 'JavaScript'};
print(set.contains('Dart')); // true
print(set.containsAll(['Java', 'JavaScript'])); // true
```

## 长度

### 长度`length`

```dart
Set<String> set = {"OC", "Swift", "Dart"};
print(set.length); // 3
```

### 空判断`isEmpty` `isNotEmpty`

```dart
Set<String> set = {};
print(set.isEmpty); // true
print(set.isNotEmpty); // false
```

## 添加元素

### 添加`add`

```dart
Set<String> set = Set();
set.add('OC');
set.add('Swift');
set.add('Swift');
print(set); // {OC, Swift}
```

### 批量添加`addAll`

```dart
Set<String> set = {};
set.addAll(['OC', 'Swift', 'Dart', 'Swift']);
print(set); // {OC, Swift, Dart}
```

## 删除元素

### 删除某元素`remove`

```dart
Set<String> set = {'Swift', 'Java', 'Dart', 'JavaScript'};
set.remove('Java');
print(set); // {Swift, Dart, JavaScript}
```

### 批删除 `removeAll`

```dart
Set<String> set = {'Swift', 'Java', 'Dart', 'JavaScript'};
set.removeAll(['Java', 'Ruby', 'Swift']);
print(set); // {Dart, JavaScript}
```

### 按条件删除`removeWhere`

```dart
Set<String> set = {'Swift', 'Java', 'Dart', 'JavaScript'};
set.removeWhere((element) => element.contains('Java'));
print(set); // {Swift, Dart}
```

### 保留某些`retainAll`

```dart
Set<String> set = {'Swift', 'Java', 'JavaScript', 'Dart'};
set.retainAll(['Swift', 'Java', 'OC']);
print(set); // {Swift, Java}
```

### 按条件保留`retainWhere`

```dart
Set<String> set = {'Swift', 'Java', 'JavaScript', 'Dart'};
set.retainWhere((element) => element.contains('Java'));
print(set); // {Java, JavaScript}
```

### 清空`clear`

```dart
Set<String> set = {'Swift', 'Java', 'Dart', 'JavaScript'};
set.clear();
print(set); // {}
```

## 布尔运算

### 交集

```dart
Set<String> set1 = {'Swift', 'Java', 'JavaScript'};
Set<String> set2 = {'Dart', 'JavaScript'};
print(set2.intersection(set1)); // {JavaScript}
```

### 并集

```dart
Set<String> set1 = {'Swift', 'Java', 'JavaScript'};
Set<String> set2 = {'Dart', 'JavaScript'};
print(set2.union(set1)); // {Dart, JavaScript, Swift, Java}
```

### 补集

```dart
Set<String> set1 = {'Swift', 'Java', 'JavaScript'};
Set<String> set2 = {'Dart', 'JavaScript'};
print(set2.difference(set1)); // {Dart}
```

## 遍历集合

### `forEach`

```dart
Set<String> set = {'Swift', 'Java', 'JavaScript', 'Dart'};
set.forEach((element) {
	print(element);
});
```







