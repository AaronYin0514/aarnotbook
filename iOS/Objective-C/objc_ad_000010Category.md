---
tags: ios, objc
---

# Category

## Category底层结构

定义在**objc-runtime-new.h**中

![](objc_category.png)

源码解读顺序：

**objc-os.mm**

* \_objc\_init
* map\_images
* map\_images_nolock

**objc-runtime-new.mm**

* \_read\_images
* remethodizeClass
* attachCategories
* attachLists
* realloc、memmove、memcpy

## Category的加载处理过程

1. 通过Runtime加载某个类的所有Category数据
2. 把所有Category的方法、属性、协议数据，合并到一个大数组中，后参与编译的Category数据会在数组的前面
3. 将合并后的分类数据(方法、属性、协议)，插入到类原来数据的前面

## +load方法

`+load`方法会在runtime加载类、分类时调用，每个类、分类的`+load`，在程序运行过程中只调用一次。

### 调用顺序

1. 先调用类的`+load`
	* 按照编译先后顺序调用（先编译，先调用）
	* 调用子类的`+load`之前会先调用父类的`+load`
1. 再调用类的`+load`
	* 按照编译先后顺序调用（先编译，先调用）

> `+load`方法是根据方法地址直接调用，并不是经过`objc_msgSend`函数调用

### 源码解读

**objc4**源码解读过程：objc-os.mm

* \_objc\_init
* load\_images
* prepare\_load\_methods
	* schedule\_class\_load
	* add\_class\_to\_loadable\_list
	* add\_category\_to\_loadable\_list
* call_load_methods
	* call\_class\_loads
	* call\_category_loads
	* (\*load\_method)(cls, SEL\_load)

## +initialize方法

`+initialize`方法会在类第一次接收到消息时调用

### 调用顺序

* 先调用父类的`+initialize`，在调用子类的`+initialize`
* 先初始化父类，在初始化子类，每个类只会初始化1次

### 源码解读

* objc-msg-arm64.s
	* objc\_msgSend
* objc-runtime-new.mm
	* class\_getInstanceMethod
	* lookUpImpOrNil
	* lookUpImpOrForward
	* \_class\_initialize
	* callInitialize
	* objc\_msgSend(cls, SEL\_initialize)

### +load方法和+initialize方法的区别

* `+initialize`和`+load`的很大却别是，`+initialize`是通过`objc_msgSend`进行调用的，所以有一下特点
	* 如果子类没有实现`+initialize`，会调用父类的`+initialize`（所以父类的`+initialize`可能会被调用多次）
	* 如果分类实现类`+initialize`，就覆盖类本身的`+initialize`调用










