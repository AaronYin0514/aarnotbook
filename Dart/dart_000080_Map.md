---
tags: dart map
---

# Map

## 创建

```dart
Map map ={
	"name": "张三",
	"age": 10,
};

print(map); // {name: 张三, age: 10}
```

### 强类型

```dart
Map<String, Object> map = Map();
map['name'] = '张三';
map['age'] = 10;
print(map); // {name: 张三, age: 10}
```

## 访问

### 访问

```dart
Map<String, Object> map = {"name": "张三", "age": 10};
print(map['name']); // 张三
print(map['age']); // 10
```

### 获取所有keys

```dart
Map<String, Object> map = {"name": "张三", "age": 10};
print(map.keys); // (name, age)
```

### 获取所有values

```dart
Map<String, Object> map = {"name": "张三", "age": 10};
print(map.values); // (张三, 10)
```

### 获取entries

```dart
Map<String, Object> map = {"name": "张三", "age": 10};
print(map.entries); // (MapEntry(name: 张三), MapEntry(age: 10))
```

## Map长度

### 长度 `length`

```dart
Map<String, Object> map = {"name": "张三", "age": 10};
print(map.length); // 2
```

### 空判断 `isEmpty` `isNotEmpty`

```dart
Map<String, Object> map = {};
print(map.isEmpty); // true
print(map.isNotEmpty); // false
```

### 判断是否包含某key `containsKey`

```dart
Map<String, Object> map = {"name": "张三", "age": 10};
print(map.containsKey("name")); // true
print(map.containsKey("description")); // false
```

### 判断是否包含某value `containsValue`

```dart
Map<String, Object> map = {"name": "张三", "age": 10};
print(map.containsValue("张三")); // true
print(map.containsValue("李四")); // false
```

## 增加键值对

### 添加键值对`[]=`

```dart
Map<String, Object> map = Map();
map['name'] = '张三';
map['age'] = 10;
print(map); // {name: 张三, age: 10}
```

### 批量添加`addAll`

```dart
Map<String, Object> map = {};
map.addAll({"name": "张三", "age": 10});
print(map); // {name: 张三, age: 10}
```

### 批量添加`addEntries`

```dart
Map<String, Object> map = {"name": "张三", "age": 10};
Map<String, Object> map1 = {};
map1.addEntries(map.entries);
print(map1); // {name: 张三, age: 10}
```

## 删除键值对

### 通过key删除`remove`

```dart
Map<String, Object> map = {"name": "张三", "age": 10};
map.remove('age');
print(map); // {name: 张三}
```

### 通过条件删除`removeWhere`

```dart
Map<String, Object> map = {"name": "张三", "age": 10};
map.removeWhere((key, value) => key == 'age');
print(map); // {name: 张三}
```

### 清除`clear`

```dart
Map<String, Object> map = {"name": "张三", "age": 10};
map.clear();
print(map); // {}
```

## 修改键值对

### 修改键值对`[]=`

```dart
Map<String, Object> map = {"name": "张三", "age": 10};
map['name'] = '李四';
print(map); // {name: 李四, age: 10}
```

### 根据key修改

```dart
Map<String, dynamic> map = {"name": "张三", "age": 10};
map.update('age', (value) => (value + 10));
print(map); // {name: 张三, age: 20}
```

### 批量修改

```dart
Map<String, dynamic> map = {"name": "张三", "age": 10};
map.updateAll((key, value) => '${key}_${value}');
print(map); // {name: name_张三, age: age_10}
```











