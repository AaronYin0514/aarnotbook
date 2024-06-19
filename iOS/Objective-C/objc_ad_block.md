---
tags: ios, objc
---

# Block

## block底层结构

![](objc_block_struct.png)

## block的变量捕获（capture）

为了保证block内部能够正常访问外部的变量，block有个变量捕获机制

| 变量类型         | 捕获到block内部 | 访问方式 |
| ---------------- | :---------------: | -------- |
| 局部变量(**auto**) | ✅              | 值传递   |
| 局部变量(**static**) | ✅              | 指针传递 |
| 全局变量         | ❌              | 直接访问 |

### auto变量的捕获

![](block_auto_capture.png)

## block的类型

block有3种类型，可以通过调用class方法或者isa指针查看具体类型，最终都继承自NSBlock类型

| block类型                                       | 环境             |
| ----------------------------------------------- | ---------------- |
| `__NSGlobalBlock__ (_NSConcreteGlobalBlock)` | 没有访问auto变量 |
| `__NSStackBlock__ (_NSConcreteStackBlock)`   | 访问了auto变量   |
| `__NSMallocBlock__ (_NSConcreteMallocBlock)` | `__NSStackBlock__`调用了`copy` |

每一种类型的block调用`copy`后的结果如下所示

| Block的类               | 副本源的配置存储域 | 复制效果     |
| ----------------------- | ------------------ | ------------ |
| `_NSConcreteStackBlock`  | 栈                 | 从栈复制到堆 |
| `_NSConcreteGlobalBlock` | 程序的数据区域     | 什么也不做   |
| `_NSConcreteMallocBlock` | 堆                 | 引用计数增加 |

## block的copy

在**ARC**环境下，编译器会根据情况自动将**栈**上的block**复制到堆上**，比如一下情况

* block作为函数返回值
* 将block赋值给`__strong`指针时
* block作为Cocoa API中方法名含有`usingBlock`的方法参数时
* block作为GCD API的方法参数时

MRC下block属性的建议写法

```objc
@property (copy, nonatomic) void (^block)(void);
```

ARC下block属性的建议写法

```objc
@property (strong, nonatomic) void (^block)(void);
@property (copy, nonatomic) void (^block)(void);
```

## 对象类型的auto变量

* 当block内部访问了对象类型的`auto`变量时
	* 如果block是在栈上，将不会对`auto`变量产生强引用

* 如果block被拷贝到堆上
	* 会调用block内部的`copy`函数
	* `copy`函数内部会调用`_Block_object_assign`函数
	* `_Block_object_assign`函数会根据`auto`变量的修饰符(`__strong`、`__weak`、`__unsafe_unretained`)做出相应的操作，形成强引用(`retain`)或弱引用

* 如果block从堆上移除
	* 会调用block内部的`dispose`函数
	* `dispose`函数内部会调用`_Block_object_dispose`函数
	* `_Block_object_dispose`函数会自动释放引用的`auto`变量(`release`)

| 函数          | 调用时机              |
| ------------- | --------------------- |
| `copy`函数    | 栈上的Block复制到堆时 |
| `dispose`函数 | 堆上的block被废弃时   |

## `__block`修饰符

`__block`可以用于解决block内部无法修改`auto`变量值的问题。`__block`不能修饰全局变量、静态变量(`static`)。

### `__block`底层结构

编译器会将`__block`变量包装称一个结构体。

```objc
__block int age = 10;

^ {
	NSLog(@"%d", age);
}();
```

编译后：

```objc
struct __Block_byref_age_0 {
	void *__isa;
	__Block_byref_age_0 *__forwarding;
	int __flags;
	int __size;
	int age;
};

struct __main_block_impl_0 {
	struct __block_impl impl;
	struct __main_block_desc_0* Desc;
	__Block_byref_age_0 *age; // by ref
};
```

![](/assets/imgs/ios/objc__block_struct.png)

### `__block`的内存管理

当block在栈上时，不会对`__block`变量产生强引用。

* 当block被copy到堆时
	* 会调用block内部的`copy`函数
	* `copy`函数内部会调用`_Block_object_assign`函数
	* `_Block_object_assign`函数会对`__block`变量形成强引用(`retain`)

![](objc__block_copy.png)

* 当block从堆中移除时
	* 会调用block内部的`dispose`函数
	* `dispose`函数内部会调用`_Block_object_dispose`函数
	* `_Block_object_dispose`函数会自动释放引用的`__block`变量(`release`)

![](objc__block_dispose.png)

### `__block`的`__forwarding`指针

![](objc__block_forwarding.png)

## 循环引用

### ARC解决循环引用

用`__weak`、`__unsafe_unretained`解决

```objc
__weak typeof(self) weakSelf = self;
self.block = ^{
	printf("%p", weakSelf);
};
```

```objc
__unsafe_unretained id weakSelf = self;
self.block = ^{
	printf("%p", weakSelf);
};
```

用`__block`解决(必须要调用block)

```objc
__block id weakSelf = self;
self.block = ^{
	printf("%p", weakSelf);
	weakSelf = nil;
};
self.block();
```

### MRC解决循环引用

用`__unsafe_unretained`解决

```objc
__unsafe_unretained id weakSelf = self;
self.block = ^{
	printf("%p", weakSelf);
};
```

用`__block`解决

```objc
__block id weakSelf = self;
self.block = ^{
	printf("%p", weakSelf);
};
```

