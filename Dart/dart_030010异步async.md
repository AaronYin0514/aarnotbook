---
tags: dart async
---

# 异步async await

## `async` `await`

```dart
Future test() {
	return Future.delayed(Duration(seconds: 2), () {
		return "response";
	});
}

Future testAsync() async {
	var result = await test();
	return 'test async $result';
}

testAsync().then((value) {
	print(value);
});
```

## 异常处理

```dart
Future test() {
	return Future.delayed(Duration(seconds: 2), () {
		throw AssertionError('test error');
	});
}

Future testAsync() async {
	var result = await test();
	return 'test async $result';
}

void main() async {
	try {
		var res = await testAsync();
		print(res);
	} catch (err) {
		print('- Error: $err');
	}
}
```