---
tags: dart async
---

# 异步`Future`

## 创建Future

`Future`的创建方式：

* `Future(task函数)`：异步执行task任务
* `Future.sync(task函数)`：同步task任务
* `Future.delayed(延时时间, task任务)`：延迟执行task任务

```dart
// 异步任务
Future(() {
	print('task 1');
});

// 同步任务
Future.sync(() {
	print('task 2');
});

// 延时任务
Future.delayed(Duration(seconds: 2), () {
	print('task 3');
});
```

结果：

```
task 2
task 1
task 3
```

## Future回调

### 成功回调`then`

```dart
Future.delayed(Duration(seconds: 2), () {
	return "Success Response";
}).then((value) => print(value));
```

### 失败回调

#### `catchError`

```dart
Future.delayed(Duration(seconds: 2), () {
	throw AssertionError('Response Error');
}).then((value) {
	print(value);
}).catchError((err) {
	print('Error: $err');
});
```

#### `then`

```dart
Future.delayed(Duration(seconds: 2), () {
	throw AssertionError('Response Error');
}).then((value) {}, onError: (err) {
	print(err);
});
```

### 完成回调

```dart
Future.delayed(Duration(seconds: 2), () {
	return 'Response';
}).then((value) {}, onError: (err) {
	print(err);
}).whenComplete(() {
	print('response complete');
});
```

## Future.wait

`wait`多任务，所有任务都成功，才成功。

```dart
Future.wait([
	Future.delayed(Duration(seconds: 2), () {
		return 'reponse 1';
	}),
	Future.delayed(Duration(seconds: 4), () {
		return 'response 2';
	})
]).then((value) {
	print(value);
}).catchError((err) {
	print(err);
});
```










