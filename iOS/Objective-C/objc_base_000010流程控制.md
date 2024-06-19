---
tags: [objc]
---

# 流程控制

## 条件判断

### if-else

```objc
int x;
int y;
char operator;

printf("请输入运算符: ");
scanf("%c", &operator);
printf("请输入第一个数字: ");
scanf("%d", &x);
printf("请输入第二个数字: ");
scanf("%d", &y);

float res = 0;

if (operator == '+') {
	res = x + y;
} else if (operator == '-') {
	res = x - y;
} else if (operator == '*') {
	res = x * y;
} else if (operator == '/') {
	res = x / y;
} else {
	NSLog(@"未能识别的运算符");
}

NSLog(@"结果：%f", res);
```

### switch-case

```objc
int x;
int y;
char operator;

printf("请输入运算符: ");
scanf("%c", &operator);
printf("请输入第一个数字: ");
scanf("%d", &x);
printf("请输入第二个数字: ");
scanf("%d", &y);

float res = 0;

switch (operator) {
	case '+':
		res = x +y;
		break;
	case '-':
		res = x -y;
		break;
	case '*':
		res = x * y;
		break;
	case '/':
		res = x / y;
		break;
	default:
		NSLog(@"未能识别的运算符");
		break;
}

NSLog(@"结果：%f", res);
```

### switch-case-break

```objc   
int x;
int y;
char operator;

printf("请输入运算符: ");
scanf("%c", &operator);
printf("请输入第一个数字: ");
scanf("%d", &x);
printf("请输入第二个数字: ");
scanf("%d", &y);

float res = 0;
switch (operator) {
	case '+':
	case 'a':
		res = x +y;
		break;
	case '-':
	case 's':
		res = x -y;
		break;
	case '*':
	case 'm':
		res = x * y;
		break;
	case '/':
	case 'd':
		res = x / y;
		break;
	default:
		NSLog(@"未能识别的运算符");
		break;
}

 NSLog(@"结果：%f", res);
```

## 循环

### for

1到100的和：

```objc
float res = 0;
for (NSInteger i = 1; i <= 100; i++) {
	res += i;
}
NSLog(@"%f", res);
```

### while

1到100的和：

```objc
float res = 0;
NSInteger i = 1;
while (i <= 100) {
	res += i;
	i++;
}
NSLog(@"%f", res);
```

### continue

`continue`跳过本次循环，进入下一次

1到100基数的和：

```objc
float res = 0;
for (NSInteger i = 1; i <= 100; i++) {
	if (i % 2 == 0) {
		continue;
	}
	res += i;
}
 NSLog(@"%f", res);
```

### break

`break`退出本次循环。

输入整数n，计算1到n的和，最高计算到100

```objc
int n = 0;
 printf("请输入n：");
 scanf("%d", &n);
 float res = 0;
 for (NSInteger i = 1; i <= n; i++) {
	 if (i > 100) {
	 	break;
	 }
	 res += i;
 }

 NSLog(@"%f", res);
```






