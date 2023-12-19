---
title: 2.3 复合类型
description: 阅读笔记，摘自《C++ Primer 5th Edition》 
slug: cpp-11-primer-2-3
date: 2023-12-19 00:01:00+0000
image: 1.png
categories:
    - C Plus Plus
tags:
    - C++
    - C++ Primer
    - C++11
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---


## 复合类型

主要分析两种复合类型：**引用**和**指针**。

### 引用

+ 引用等价于为对象起了**别名**，通过将声明符写作`&variable`的形式来定义引用类型。
+ 定义引用时，**初始值与引用绑定在一起**，而不是将初始值拷贝到新建的引用中。
+ 引用必须**初始化**，并且一旦引用完成初始化，便**无法**与另一个对象重新绑定。

#### 引用即别名

+ **引用不是一个对象**，所有对引用的操作都是在与之绑定的对象上进行。

```c++
int v = 1024;
int &ref = v;      // 正确，v的值与ref进行绑定
int &ref2;         // 错误，引用必须初始化

ref = 2;           // 等同于 "v = 2;"
int v2 = ref;      // 等同于 "int v2 = v;"
int &ref2 = ref;   // 等同于 "int &ref2 = v;"
```

+ **不能定义引用的引用**。

```c++
int & &x = v;   // 错误，不能引用未分配内存的对象，即不能定义引用的引用
```

#### 引用的定义

+ 允许一条语句中定义多个引用，但每个引用必须以`&`开头。
+ 除部分特例，**引用的类型须与其绑定的对象严格匹配**。
+ 引用只能绑定在对象上，不能是字面值或者表达式结果。

```c++
int i1 = 1024, i2 = 2048;
int &r1 = i1, &r2 = i2;  // 声明r2引用时必须用&开头

int &r3 = 10;            // 错误，引用只能与对象绑定，不能是字面值
double f1 = 3.14;
int &r4 = f1;            // 错误，引用的类型必须和绑定的对象保持一致
```

#### 指针

+ 定义指针类型将声明符写成`*variable`的形式，如果在一个语句中定义多个指针，则每个指针变量都以`*`开头。
+ 在**块作用域**内定义的指针如果未初始化，则是**未定义**的。
+ 指针和引用都能提供对其他对象的**间接访问**。

#### 指针与引用的区别

+ 指针本身就是一个**对象**，引用不是。
+ 指针在生命周期内可以**先后指向不同对象**，引用则只能与对象绑定一次。
+ 指针无须在定义时**初始化**，引用必须初始化。

#### 获取对象的地址

+ 取地址符`&`：获取**对象的地址**，保存在指针之中。
+ 引用不是对象，所以**不可以定义指向引用的指针**。
+ 除了部分特例，**指针的类型需与其指向的对象严格匹配**。

```c++
double d;
double *pd1 = &d;   // 正确，初始化一个指向变量d的double型指针
double *pd2 = pd1;  // 正确，同样初始化一个指向变量d的指针
int *pi1 = &d;      // 错误，指针类型与所指向的对象不匹配
```

#### 指针值

+ 指针的值应属于下列4种状态之一：

  + 指向一个对象；

  + 指向紧邻对象所占空间的下一个位置；

  + 空指针：不指向任何对象；

  + 无效指针。

+ 试图拷贝或以其他方式访问**无效指针**将引发错误，并且编译器不负责检查此类错误(即不会发出warning或者报错)

#### 利用指针访问对象

+ **解引用符`*`**：作用于指针，用以访问其**所指向的对象**。
+ 对指针解引用的结果进行赋值，本质上就是**对指针指向的对象赋值**。

```C++
int i = 42;
int *p = &i;
*p = 0;       // 等价于将变量i赋值为0
```

+ 解引用前必须确认该指针确实指向了一个对象。

#### 空指针

+ 空指针不指向任何对象，在使用一个指针之前最好检查其是否为空指针。
+ 生成空指针的若干方法：

```c++
int *p1 = nullptr;  // 等价于 int *p1 = 0;
int *p2 = 0;        // 必须是字面值常量0，不能是值为0的int型变量

#include <cstdlib>
int *p3 = NULL;     // 等价于 int *p3 = 0;
```

+ 不能将int型变量直接赋值给指针，即便该变量的值为0。

#### 赋值和指针

+ 指针可以反复赋值，而引用则不行。
+ 注意区分赋值时，等号左边的是指针所指向的对象还是指针自身。

```C++
int i = 42;
int *p = 0;  // 初始化整型指针p，将其置为空指针
p = &i;      // 将指针p赋值为i的地址，使其指向变量i
*p = 0;      // 对指针p所指向的对象i进行赋值，等价于 i = 0;
p = 0;       // 对指针p进行赋值，将其重置为空指针
```

#### 其他指针操作

+ 如果指针的值是0(即空指针)，则其对应的条件值为`false`，反之则为`true`。
+ 可以用相等操作符比较两个**类型相同的合法**指针，比较的结果是布尔类型，比较的内容则是二者存放的地址。若两指针相等，则：
  + 都为空指针；
  + 指向同一个对象；
  + 对象同一个对象的下一个地址；
  + (可能出现) 一个指针指向某对象，另一指针指向另一个对象的下一地址。

#### void* 指针

+ 用于存放**任意对象的地址**。
+ 不能直接操作`void*`指针所指向的对象，因不知其所指向的对象的类型。

```C++
double obj = 3.14, *pd = &obj;

void *pv = &obj;        // 可以指向任意类型的对象
cout << *(double*)pv;   // 不能直接打印pv的值，必须首先将其转化为固定类型的指针

pv = &pd;               // 可以指向任意类型的指针(指针自身也是一个对象)
cout << **(double**)pv; // 双重指针可以两次解引用，结果为pd指向的变量obj(值为3.14)
```

### 理解复合类型的声明

#### 定义多个变量

+ 变量的定义包含**一个基本数据类型**和**一组声明符**，即在一条语句中，**基本数据类型只有一个**，但是声明符的形式可以不同。
+ **类型修饰符属于声明符的一部分**，因此一条语句可以定义出不同类型的结果。

```C++
int i = 1024, *p = &i, &r = i; 
// 其中(*p = &i)整体为一个声明符，(&r = i)为另一个，声明符之间用逗号隔开
```

+ 指针和引用的声明有两种写法，一种是将修饰符与**变量**写在一起，另一种是将修饰符与**类型名**写在一起。

```C++
int *p1;
int* p1;
```

#### 指向指针的指针

+ 指针修饰符可以**叠加**，用以表示存放指针地址的指针，即**指向指针的指针**。

```C++
int i = 1024;
int *p = &i;
int **pp = &p; 
// 声明一个指向指针p的指针pp，通过pp指针访问i的值时，需要两次解引用**pp
```

#### 指向指针的引用

+ 指针是一个**对象**，因此可以定义**对指针的引用**

```C++
int i = 42;
int *p = &i;
int *&r = p;  // 声明对指针p的引用

r = &i;       // 使得p指针指向i，等价于 p = &i;
*r = 0;       // 修改p指针指向对象的值，等价于 *p = 0;
r = 0;        // 将指针p重置为空指针。
```

+ 面对多重指针和引用的复合类型，可以**从右向左**的阅读其修饰符列表，**距离变量名最近的修饰符**对变量有**最直接的影响**(若为`&`则是引用，为`*`就表示指针)，其余部分则用以**确定其指向/引用的类型**。                                                                                               