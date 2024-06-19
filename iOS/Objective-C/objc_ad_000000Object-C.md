---
tags: objc, ios
---

# Objective-C
## Objective-C的本质

```
  -------------       -------       ---------      ----------
 | Objective-C | --> | C\C++ | --> | 汇编语言 | --> | 机器语言 |
  -------------       -------       ---------      ----------
```

Objective-C代码低层是基于C\C++的数据结构(结构体)实现的。

将Objective-C的代码转换为C\C++代码

```shell
xcrun -sdk iphoneos clang -arch arm64 -reweite-objc OC源文件 -o 输出的CPP文件
```

如果需要链接其它框架，使用`-framework`参数。比如`-framework UIKit`

## OC对象的本质

![](objc_impl.png)

ObjC对象内存布局

![](objc_instance.png)

![](objc_obj.png)

### Xcode实时查看内存数据

```
Debug -> Debug Workfllow -> View Memory (Shift + Command + M)
```

### 查看对象内存占用函数

查看创建一个实例对象，**至少需要多少内存**？

```objc
[[import]] <objc/runtime.h>
class_getInstanceSize([NSObject class]);
```

查看创建一个实例对象，**实际分配多少内存**？

```objc
[[import]] <malloc/malloc.h>
malloc_size((__bridge const void *)obj);
```

### OC对象的分类

* instance对象（**实例**对象）
* class对象（**类**对象）
* meta-class对象（**元类**对象）

![](objc_struct.png)

##### 获取实例的类对象

```objc
NSObject *object1 = [[NSObject alloc] init];
Class objectClass1 = [object1 class];
Class objectClass2 = [NSObject class];
Class objectClass3 = object_getClass(object1); // Runtime API
```

##### 获取类对象的元类对象

```objc
Class objectMetaClass = object_getClass([NSObject class]); // Runtime API
```

##### 查看Class是否为meta-class

```objc
BOOL result = class_isMetaClass([NSObject class]); // Runtime API
```

### isa指针

![](objc_isa.png)

![](objc_isa_tu.png)

> 从64bit开始，isa需要进行一次位运算，才能计算出真实地址

```objc
# if __arm64__
#    define ISA_MASK    0x0000000ffffffff8ULL
# elif __x86_64__
#    define ISA_MASK    0x00007ffffffffff8ULL
```

### objc_class结构

下载[objc4源码](https://opensource.apple.com/tarballs/objc4/)

```objc
typedef struct objc_class *Class;
```

![](objc_class.png)

## 常用LLDB指令

| 命令                                  | 说明           | 例子                      |
| ------------------------------------- | -------------- | ------------------------- |
| `print` 或 `p`                        | 打印           |                           |
| `po`                                  | 打印对象       |                           |
| `memory read/数量格式字节数 内存地址` | 读取内存       |    |
| `x/数量格式字节数 内存地址`           | 读取内存       | `x/3xw 0x10010`   |
| `memory write 内存地址 数值`          | 修改内存中的值 | `memory write 0x0000010 10` |                       |

> **格式**： `x`是16进制，`f`是浮点，`d`是10进制

> **字节大小**： `b` byte 1字节，`h` half word 2字节，`w` word 4字节，`g` giant word 8字节

## KVO

## KVC

## Category

## 关联对象

## Block

