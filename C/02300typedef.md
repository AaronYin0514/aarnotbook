# typedef

这讲介绍C语言中很常用的一个关键字---typedef。

## 一、typedef作用简介

* 我们可以使用typedef关键字为各种数据类型定义一个新名字(别名)。

```c
[[include]] <stdio.h>

typedef int Integer;
typedef unsigned int UInterger;

typedef float Float;

int main(int argc, const char * argv[]) {
    Integer i = -10;
    UInterger ui = 11;

    Float f = 12.39f;

    printf("%d  %d  %.2f", i, ui, f);

    return 0;
}
```

在第3、第4、第6行分别给int、unsigned int、float起了个别名，然后在main函数中使用别名定义变量，用来跟原来的基本类型是完全一样的。输出结果：

![](https://images0.cnblogs.com/blog/497279/201303/24182743-fb01198b097a49d7b26ff847e46be9cf.png)

当然，给类型起别名后，原来的int、float还是可以正常使用的：

```c
int i = 10; 
float f = 10.0f;
```

* 也可以在别名的基础上再起一个别名

```c
typedef int Integer;

typedef Integer MyInteger;
```

## 二、typedef与指针

除开可以给基本数据类型起别名，typedef也可以给指针起别名

```c
[[include]] <stdio.h>

typedef char *String;

int main(int argc, const char * argv[]) {
    // 相当于char *str = "This is a string!";
    String str = "This is a string!";

    printf("%s", str);

    return 0;
}
```

在第3给指针类型`char *`起别名为String，然后在第7行使用String定义了一个字符串，是不是有点Java的感觉？

## 三、typedef与结构体

给结构体起别名可以使代码更加简洁明

### 1.默认情况下结构体变量的使用

```c
// 定义一个结构体
struct MyPoint {
    float x;
    float y;
};

int main(int argc, const char * argv[]) {
    // 定义结构体变量
    struct MyPoint p;
    p.x = 10.0f;
    p.y = 20.0f;

    return 0;
}
```

默认情况下，我们定义结构体变量需要带个struct关键字，看第9行

### 2.使用typedef给结构体起别名

```c
// 定义一个结构体
struct MyPoint {
    float x;
    float y;
};

// 起别名
typedef struct MyPoint Point;

int main(int argc, const char * argv[]) {
    // 定义结构体变量
    Point p;
    p.x = 10.0f;
    p.y = 20.0f;

    return 0;
}
```

我们在第8行给结构体MyPoint起了个别名叫做Point，然后在12行使用Point定义了一个结构体变量p，不用再带上struct关键字了

其实第1~第8行的代码可以简写为：

```c
// 定义一个结构体，顺便起别名
typedef struct MyPoint { 
  float x; 
  float y;
} Point;
```

甚至可以省略结构体名称：

```c
typedef struct { 
  float x; 
  float y;
} Point;
``` 

## 三、typedef与指向结构体的指针

typedef可以给指针、结构体起别名，当然也可以给指向结构体的指针起别名

```c
[[include]] <stdio.h>

// 定义一个结构体并起别名
typedef struct {
    float x;
    float y;
} Point;

// 起别名
typedef Point *PP;

int main(int argc, const char * argv[]) {
    // 定义结构体变量
    Point point = {10, 20};

    // 定义指针变量
    PP p = &point;

    // 利用指针变量访问结构体成员
    printf("x=%f，y=%f", p->x, p->y);
    return 0;
}
```

在第4行定义了一个结构体，顺便起了个别名叫Point，第10行为指向结构体的指针定义了别名PP。然后在main函数中使用这2个别名。

输出结果：

![](https://images0.cnblogs.com/blog/497279/201303/24190016-d86d64d1fd3e4859964aa4651fbcb690.png)

## 四、typedef与枚举类型

使用typedef给枚举类型起别名也可以使代码简洁。

```c
// 定义枚举类型
enum Season {spring, summer, autumn, winter};
// 给枚举类型起别名
typedef enum Season Season;

int main(int argc, const char * argv[]) {
    // 定义枚举变量
    Season s = spring;

    return 0;
}
```

在第2行定义了枚举类型，在第4行起了别名为Season，然后在第8行直接使用别名定义枚举变量，不用再带上enum关键字了。

第1行~第4行代码可以简化为：

```c
// 定义枚举类型，并且起别名
typedef enum Season {spring, summer, autumn, winter} Season
```

甚至可以省略枚举名称，简化为：

```c
typedef enum {spring, summer, autumn, winter} Season;
```

## 五、typedef与指向函数的指针

1.先来回顾下函数指针的知识

```c
[[include]] <stdio.h>

// 定义一个sum函数，计算a跟b的和
int sum(int a, int b) {
    int c = a + b;
    printf("%d + %d = %d", a, b, c);
    return c;
}

int main(int argc, const char * argv[]) {
    // 定义一个指向sum函数的指针变量p
    int (*p)(int, int) = sum;

    // 利用指针变量p调用sum函数
    (*p)(4, 5);

    return 0;
}
```

* 在第4行定义了一个sum函数，第12行定义了一个指向sum函数的指针变量p，可以发现，这个指针变量p的定义比一般的指针变量看来复杂多了，不利于理解。

* 第15行调用了p指向的sum函数，输出结果：

![](https://images0.cnblogs.com/blog/497279/201303/24214838-5b322689736d486b8c487d6678217463.png)

2.为了简化代码和方便理解，我们可以使用typedef给指向函数的指针类型起别名

```c
[[include]] <stdio.h>

// 定义一个sum函数，计算a跟b的和
int sum(int a, int b) {
    int c = a + b;
    printf("%d + %d = %d", a, b, c);
    return c;
}

typedef int (*MySum)(int, int);

int main(int argc, const char * argv[]) {
    // 定义一个指向sum函数的指针变量p
    MySum p = sum;

    // 利用指针变量p调用sum函数
    (*p)(4, 5);

    return 0;
}
```

* 看第10行，意思是：给指向函数的指针类型，起了个别名叫MySum，被指向的函数接收2个int类型的参数，返回值为int类型。
* 在第14行直接用别名MySum定义一个指向sum函数的指针变量p，这样看起来简单舒服多了。第17行的函数调用是一样的。

## 六、typedef与#define

1.先来看看下面的两段代码有什么区别\(注意每一段的第1行代码\)

* 第1段

```c
typedef char *String;  
int main(int argc, const char * argv[]) { 
  String str = "This is a string!"; 
  return 0;
}
```

* 第2段

```c
[[define]] String char *

int main(int argc, const char * argv[]) { 
  String str = "This is a string!"; 
  return 0; 
}
```

上面的两段代码只是第1行代码不一样，运行的效果都是一样的：定义了一个字符串"This is a string!"。

但它们的实现方式是不一样的：

* 第1段代码是用typedef给`char *`定义别名为String
* 第2段代码是用`char *`代替代码中的宏名String

只看上面两段代码，似乎看不太出typedef和#define的区别。

2.再来看一段代码

```c
typedef char *String1;

[[define]] String2 char *

int main(int argc, const char * argv[]) {
    String1 str1, str2;

    String2 str3, str4;
    return 0;
}
```

第1行给`char *`起了个别名String1，第2行定义了宏String2。然后在第6、第8行定义了4个变量。

重点来了，注意：在这种情况下，只有str1、str2、str3才是指向char类型的指针变量，str4只是个char类型的变量。

下面简单分析一下原因：

* 如果连续声明两个int类型的变量，我们可以这样写：

```c
int a, b;
```

上面的代码相当于：

```c
int a; int b;
```

* 以此类推

```c
typedef char *String1; 
   
String1 str1, str2;
```

经过typedef处理后，String1也算是一种数据类型，所以第3行代码相当于

```c
String1 str1; 
String1 str2;
```

由于String1就是`char *`，所以上面的两行代码等于

```c
char *str1; 
char *str2;
```

* 再看看宏定义的情况

```c
[[define]] String2 char *

String2 str3, str4;
```

因为宏定义纯粹是字符串替换，用`char *`代替String2，所以第3行代码相当于

```c
char * str3, str4;
```

其实也就相当于：

```c
char * str3; 
char str4;
```

可以看出，只有str4是基本数据类型，str1、str2、str3都是指针类型。

所以，以后给类型起别名，最好使用typedef，而不是使用#define