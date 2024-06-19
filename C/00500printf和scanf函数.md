# printf和scanf函数

## 一、printf函数

这是在stdio.h中声明的一个函数，因此使用前必须加入`#include <stdio.h>`，使用它可以向标准输出设备（比如屏幕）输出数据

### 1.用法

**1>** printf(字符串)

```c
printf("Hello, World!");
```

输出结果是:

![](https://images0.cnblogs.com/blog/497279/201303/14170851-7cdbded5647142d1bd486533c4640ed2.png)

**2>** printf(字符串, 格式符参数)

```c
// 使用常量作参数
printf("My age is %d\n", 26);

// 也可以使用变量
int age = 17;
printf("My age is %d", age);
```

* 格式符\%d表示以有符号的十进制形式输出一个整型，格式符参数中的26和age会代替\%d的位置。
* 第2行代码中的\\n是个转义字符，表示换行，所以输出了第一句"My age is 26"后会先换行，再输出"My age is 27"

输出结果：
![](https://images0.cnblogs.com/blog/497279/201303/14171725-45f8872e92e74314b0e72aaf3a3451a9.png)

* 如果去掉第2行中的\\n，将会是这样的效果

输出结果：

![](https://images0.cnblogs.com/blog/497279/201303/14173536-d8c26ca8767149d3b3d31b6835cdde85.png)

总结：左边字符串中格式符的个数 必须跟 右边格式符参数的个数一样；格式符的类型决定了格式符参数的类型，比如使用\%d，说明对应的格式符参数必须是整型。

再举个例子：

```c
printf("My age is %d and no is %d", 27, 1);
```

输出结果：

![](https://images0.cnblogs.com/blog/497279/201303/14172957-b35dc0fb9beb42e58841436eab6fe29f.png)

### 2.常用的格式符及其含义

![](https://images0.cnblogs.com/blog/497279/201303/14172422-a1d495a292ff4440a7c86e1bbd1cdf5d.png)

### 3.格式符还可以添加一些精细的格式控制

#### 1> 输出宽度

* 我们先看看默认的整型输出

```c
printf("The price is %d.", 14);
```

输出结果(注意，后面是有个点的)：

![](https://images0.cnblogs.com/blog/497279/201303/14175246-3e9f11839af94687a77322ecacff8428.png)

* 如果我把\%d换成\%4d：

```c
printf("The price is %4d.", 14);
```

输出结果：

![](https://images0.cnblogs.com/blog/497279/201303/14175314-b5596e37395d4754b33213cb52429bf7.png)

你会发现"is"跟"14"的距离被拉开了

%4d的意思是输出宽度为4，而"14"的宽度为2，因此多出2个宽度，多出的宽度就会在左边用空格填补，因此你会看到"14"左边多了2个空格；如果实际数值宽度比较大，比如用\%4d输出宽度为6的"142434"，那就会按照实际数值宽度6来输出。

```c
printf("The price is %4d.", 142434);
```

输出结果：

![](https://images0.cnblogs.com/blog/497279/201303/14175517-7634743c977c45ddb2e3b94dfd0c67f7.png)

"142434"的输出宽度为6

* 如果换成%-4d

```c
printf("The price is %-4d.", 14);
```

输出结果：

![](https://images0.cnblogs.com/blog/497279/201303/14175745-3eeefc1fd98d45d18857f8f1eb294cfe.png)

你会发现"14"跟"."的距离被拉开了

%-4d表示输出宽度为4，如果比实际数值宽度大，多出的宽度会在右边用空格填补；如果4比实际数值宽度小，就按照实际数值的宽度来输出

#### 2> 浮点数的小数位数

* 我们先看下默认的浮点数输出

```c
printf("My height is %f", 179.95f);
```

输出结果：

![](https://images0.cnblogs.com/blog/497279/201303/14173857-6c74dacdb32b4d5e92d44065c0edf02b.png)，默认是输出6位小数

* 如果只想输出2位小数，把\%f换成\%.2f即可

```c
printf("My height is %.2f", 179.95f);
```

输出结果：![](https://images0.cnblogs.com/blog/497279/201303/14174017-98f990ef2ba3459da8e5b0b4bcd35d3b.png)

* 当然，可以同时设置输出宽度和小数位数

```c
printf("My height is %8.1f", 179.95f);
```

输出结果：

![](https://images0.cnblogs.com/blog/497279/201303/14180546-0b9c971ec78d4b948e87ded8598793f3.png)

输出宽度为8，保留1位小数

## 二、scanf函数

这也是在stdio.h中声明的一个函数，因此使用前必须加入`#include <stdio.h>`。调用scanf函数时，需要传入变量的地址作为参数，scanf函数会等待标准输入设备（比如键盘）输入数据，并且将输入的数据赋值给地址对应的变量

### 1.简单用法

```c
printf("Please input your age:");

int age;
scanf("%d", &age);

printf("Your age is %d.", age);
```

* 运行程序，执行完第1行代码，控制台会输出一句提示信息：

![](https://images0.cnblogs.com/blog/497279/201303/14191823-fe630e38ad8240cd8e89c6e2d3f41e0a.png)

* 执行到第4行的scanf函数时，会等待用户的键盘输入，并不会往后执行代码。scanf的第1个参数是"%d"，说明要求用户以10进制的形式输入一个整数。这里要注意，scanf的第2个参数传递的不是age变量，而是age变量的地址\&age，\&是C语言中的一个地址运算符，可以用来获取变量的地址。

* 接着我们可以在提示信息后面输入个8：

![](https://images0.cnblogs.com/blog/497279/201303/14192044-210477fb4e3e4c05ae2b13ebb070e199.png)

(由于Xcode自身的问题，我们只能在控制台输入宽度为1的数据，如果想输入宽度大于1的数据，比如输入27，可以从别的地方复制个27，再粘贴到控制台)

* 输入完毕后，敲一下回车键，目的是告诉scanf函数我们已经输入完毕了，scanf函数会将输入的8赋值给age变量

* scanf函数赋值完毕后，才会往后执行代码，执行到第6行时，控制器会输出：

![](https://images0.cnblogs.com/blog/497279/201303/14201531-9b20877b3658470a87318b2a77cc9812.png)

### 2.其他用法

#### 1> 用scanf函数接收3个数值，在这里，每个数值之间用中划线-隔开

```c
int a, b, c;
scanf("%d-%d-%d", &a, &b, &c);

printf("a=%d, b=%d, c=%d", a, b, c);
```

* 注意第2行，3个\%d之间是用中划线-隔开的，因此我们在每输入一个整数后都必须加个中划线-，比如这样输入

![](https://images0.cnblogs.com/blog/497279/201303/14202751-fe5d9b5d610c45cf868ec84b489d7e45.png)

不然在给变量赋值的时候会出问题

* 所有的数值都输入完毕后敲回车键，scanf函数会依次给变量a、b、c赋值，接着输出

![](https://images0.cnblogs.com/blog/497279/201303/14203109-c6838409544f42b9a856ccb80df280b9.png)

注意：数值之间的分隔符是任意的，不一定要用中划线-，可以是逗号、空格、星号*、井号#等等，甚至是英文字母

```c
// 逗号,
scanf("%d,%d,%d", &a, &b, &c); // 输入格式：10,14,20

// 井号#
scanf("%d#%d#%d", &a, &b, &c); // 输入格式：10#14#20

// 字母x
scanf("%dx%dx%d", &a, &b, &c); // 输入格式：10x14x20
```

#### 2> 用scanf函数接收3个数值，每个数值之间用空格隔开

```c
int a, b, c;
scanf("%d %d %d", &a, &b, &c);

printf("a=%d, b=%d, c=%d", a, b, c);
```

* 注意第2行，3个\%d之间是用空格隔开的，我们在每输入一个整数后必须输入一个分隔符，分隔符可以是空格、tab、回车

* 用空格做分隔符

![](https://images0.cnblogs.com/blog/497279/201303/14221447-83c148f3a69043c890015ee309737e6e.png)

* 用tab做分隔符

![](https://images0.cnblogs.com/blog/497279/201303/14221455-722047b894534ac09e431214769352f0.png)

* 用回车做分隔符

![](https://images0.cnblogs.com/blog/497279/201303/14221513-987072db83904d199a13b0877a7805eb.png)