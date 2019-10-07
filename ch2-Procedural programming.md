## 面向过程的编程风格

### 指针(pointer)与引用(reference)

```C++
ing ival = 1024;
int *pi = &ival;  // pointer
int &rval = ival;  // reference
```

- 引用是别名，而指针是地址。指针可以被赋值，以指向另一个不同的对象，而引用总是指向在初始化时被指定的对象，以后不能修改，但引用的那个对象内容却可以改变。可以把引用理解为指针常量，而普通指针为指针变量。
- 从内存分配上来看，程序为指针变量分配内存区域，而不为引用分配内存区域。
- 在使用上，指针可能（也可能不）指向一个对象，提领时一定要先确定其值非 0。而引用，则必定会代表某个对象，所以不需要作此检查。

【ESC 46，47】

#### 引用作为函数参数

- 传递引用给函数与传递指针给函数的效果是一样的。这时，被调函数的形参就成为原来主调函数中的实参变量或对象的一个别名来使用，所以在被调函数中对形参变量的操作就是对其相应的目标对象（在主调函数中）的操作。

- 使用引用传递函数的参数，在内存中并没有产生实参的副本，它是直接对实参操作；而使用一般变量传递函数的参数，当发生函数调用时，需要给形参分配存储单元，形参变量是实参变量的副本；如果传递的是对象，还将调用拷贝构造函数。因此，当参数传递的数据较大时，用引用比用一般变量传递参数的效率和所占空间都好。
- 使用指针作为函数的参数虽然也能达到与使用引用的效果，但是，在被调函数中同样要给形参分配存储单元，且需要重复使用"*指针变量名"的形式进行运算，这很容易产生错误且程序的阅读性较差；另一方面，在主调函数的调用点处，必须用变量的地址作为实参。而引用更容易使用，更清晰。

#### 常引用作为函数参数

如果既要利用引用提高程序的效率，又要保护传递给函数的数据不在函数中被改变，就应使用常引用

```C++
void foo(const string &s) {
    cout << s << endl;
}  
// 如果形参里的 const 去掉，程序就报错，
// 因为 ss 与 "world!" 都是常量，你不能把一个const类型转换成非 const 类型。
// 所以 foo() 形参必定要用 const 修饰。
int main() {
    const string ss("hello ");
    foo(ss);
    foo("world!");
}
```

#### 函数返回引用与返回值

好处：在内存中不产生被返回值的副本；（注意：正是因为这点原因，所以返回一个局部变量的引用是不可取的。因为随着该局部变量生存期的结束，相应的引用也会失效，产生runtime error!)

【注意】

- 最好不要返回局部变量的引用；
- 最好不要返回函数内部new分配的内存的引用；
- 流操作符重载返回值应为引用；
- 全局变量和局部静态变量的返回值可以是引用；
- 可以返回类成员的引用；

[返回指针引用示例](https://blog.csdn.net/selina8921/article/details/79763169)

### 作用域与生存周期

c++ 变量有两个属性非常重要：作用域和生存周期。

#### 内存分配

一个程序将操作系统分配给其运行的内存块分为4个区域：

- 静态区（全局区）：存放程序的全局变量和静态变量。初始化的全局变量和静态变量在一个区域， 未初始化的全局变量和未初始化的静态变量在相邻的另一块区域。程序结束后由系统释放。

- 堆区：存放程序的动态数据。

- 栈区：存放程序的局部数据，即各个函数中参数和局部变量等。函数结束，自动释放。

- 文字常量区：常量字符串就是放在这里，程序结束后由系统释放。

- 代码区：存放程序的代码，即程序中的各个函数代码块。

```c++
int a = 0; //全局初始化区
char *p1;  //全局未初始化区
int main() 
{
    int b;    //栈
    char s[] = "abc"; //栈
    char *p2; //栈
    char *p3 = "123456";  //123456\0在常量区，p3在栈上。
    static int c = 0;  //全局（静态）初始化区
    p1 = (char *)malloc(10);
    p2 = (char *)malloc(20);  //分配得来的10和20字节的区域就在堆区。
    strcpy(p1, "123456");  //123456\0放在文字常量区，编译器可能会将它与p3所指向的"123456"优化成一个地方
}
```

[[C/*C++内存*分配](https://zhuanlan.zhihu.com/p/82298443)](https://zhuanlan.zhihu.com/p/82298443)

#### 局部变量和全局变量

局部变量也称为内部变量，它是在函数内定义的。其作用域仅限于函数内，离开该函数后再使用这种变量是非法的。

全局变量也称为外部变量，它是在函数外部定义的变量。它不属于哪一个函数，它属于一个源程序文件。其作用域是整个源程序。在函数内部，局部变量可以屏蔽全局变量。如果一个全局变量用 static 修饰，它就是静态全局变量，它的作用域是该文件范围（称为文件作用域）

#### 静态存储与动态存储

变量的生存周期只与变量的存储位置（存储类别）有关。可以分为：

- 静态存储方式（在程序运行期间，系统对变量分配**固定的**存储空间）
- 动态存储方式（在程序运行期间，系统对变量动态（**不固定**）的分配存储空间）

而变量的存储类别可以分为静态存储和动态存储
- auto  自动变量(动态存储方式) 
- static 静态变量(静态存储方式)  
-  register 寄存器变量(动态存储方式) 
- extern 外部变量(静态存储方式) 

[【c++】变量的作用域和生存周期](https://blog.csdn.net/u012679707/article/details/80188124)

#### C++变量保存在堆还是栈？
- 如果对象是函数内的非静态局部变量，则对象，对象的成员变量保存在栈区。
- 如果对象是全局变量，则对象，对象的成员变量保存在静态区。
- 如果对象是函数内的静态局部变量，则对象，对象的成员变量保存在静态区。
- 如果对象是new出来的，则对象，对象的成员变量保存在堆区。
```C++
void foo() {
    int* p = new int[5]; 
}
//在栈内存中存放了一个指向一块堆内存的指针p。
//在程序会先确定在堆中分配内存的大小，然后调用operator new分配内存，
//然后返回这块内存的首地址，放入栈中
```
[C/C++堆、栈及静态数据区详解](https://www.cnblogs.com/hanyonglu/archive/2011/04/12/2014212.html)

### new 与 delete

堆内存（空闲空间）里的内存分配通过 new 表达式来完成，释放通过 delete 表达式来完成。堆内存由程序员自行管理。

```C++
int *p1 = new int;  // 未初始化
int *p2 = new int(1024); // 指定初值, p2 指向一个 int 对象，其值为 1024
int *p3 = new int[1024]; // 从 heap 中分配一个数组，含有1024个元素，p3 指向数组第一个元素
delete p1;
delete p2;  //  对于普通数据类型， delete p 与 delete [] p 作用效果一样。
delete [] p3  // 复杂对象，必须用 delete [] p 来释放内存
// 如果不使用 delete，由 heap 分配而来的对象就永远不会被释放，这被称之为内存泄漏。
```

【ESC 49，50】

#### new/delete 与 malloc/free 关系

malloc/free是C/C++的标准库函数，new/delete是C++的运算符。它们都可用于申请动态内存和释放内存。对于非内部数据类型的对象而言，光用maloc/free无法满足动态对象的要求。对象在创建的同时要自动执行构造函数，对象在消亡之前要自动执行析构函数。由于malloc/free是库函数而不是运算符，不在编译器控制权限之内，不能够把执行构造函数和析构函数的任务强加于malloc/free。因此C++语言需要一个能完成动态内存分配和初始化工作的运算符new，以及一个能完成清理与释放内存工作的运算符delete

- new / new[]：完成两件事，先底层调用 malloc 分了配内存，然后调用构造函数（创建对象）。
- delete/delete[]：也完成两件事，先调用析构函数（清理资源），然后底层调用 free 释放空间。
- new 在申请内存时会自动计算所需字节数，而 malloc 则需我们自己输入申请内存空间的字节数。

**malloc/free的使用**

```C++
#include <iostream>
using namespace std;
int main() {
    int *score, num;
    cin >> num;
    if ((score = (int *)malloc(sizeof(int) * num)) == nullptr)
        cerr << "fail" << endl;
    else {
        do something;
        free(score);//释放内存
    }
}
```

### 头文件

- 头文件的扩展名习惯上是.h，标准库例外。
- 函数的定义只能有一份，倒是可以有多份声明。我们不能把函数的定义放在头文件，因为一个程序的多个代码文件可能都会包含这个头文件。但只定义一份的规则有一个例外，内联函数。为了能够扩展内联函数的内容，以便在每个调用点上，编译器都取得其定义，必须将内联函数的定义放在头文件中，而不是放在各个不同程序代码文件中。
- 一个对象和变量同函数一样，也只能在程序中定义一次，因此也应该将定义放在程序代码文件中，而不是头文件中。一般地，加上 extern 就可以放在头文件中作为声明了。

```C++
//如果想声明一个变量而非定义它，就在变量名前添加extern关键字，而且不要显式地初始化变量
extern int x;  //声明x而非定义
int y;         //声明并定义y
```

- `const int a = 6;`就可以放入头文件中，因为 const object 就和 inline 函数一样，是”一次定义“规则下的例外。因为 const 定义一出文件外便不可见（文件作用域），这意味着可以在多个不同的文件中加以定义。
- 头文件用 <> 表明此文件被认为是标准的或项目专属的头文件，编译器搜索此文件时，会先在系统默认的磁盘目录下寻找；头文件用 ” “ 表明此文件被认为是用户提供的头文件，会先在包含此文件的磁盘目录开始寻找，找不到再去系统默认的目录下寻找。

【ESC 63，64】

#### #ifndef/#define/#endif作用

防止头文件的重复包含和编译

```C++
#ifndef A_H // 意思是if not define a.h"  如果不存在a.h
#define A_H // 就引入 a.h
#include <math.h> // 引用标准库的头文件 
… 
#include “header.h” // 引用非标准库的头文件 
… 
void Function1(…); // 全局函数声明 
… 
class Box // 类结构声明 
{ 
… 
}; 
#endif  // 最后一句应该写#endif，它的作用相当于if的反花括号 '}'
```

[#ifndef/#define/#endif深刻理解](https://blog.csdn.net/u012154840/article/details/78258406)

一个类子[c/c++ 中#ifndef和#endif的作用及使用](https://www.cnblogs.com/Chicago/p/9435457.html)

### typedef 声明

可以使用 typedef 为一个已有的类型取一个新的名字。 

typedef 可以声明各种类型名，但不能用来定义变量。用 typedef 可以声明数组类型、字符串类型，使用比较方便。 

用typedef只是对已经存在的类型增加一个类型名，而没有创造新的类型。

```cpp
typedef int feet;//告诉编译器，feet 是 int 的另一个名称：可以理解 feet 为 int 的别名
feet distance;//它创建了一个整型变量 distance
```

### 枚举类型

如果一个变量只有几种可能的值，可以定义为枚举(enumeration)类型。每个枚举元素在声明时被分配一个整型值，默认从 0 开始，逐个加 1。也可以在声明时将枚举元素的值一一列举出来。

注意

- 枚举元素是常量，除了初始化时不可给它赋值。
- 枚举变量的值只可取列举的枚举元素值。

```cpp
enum 枚举名{ 
     标识符[=整型常数], 
     标识符[=整型常数], 
... 
     标识符[=整型常数]
} 枚举变量;
```

如果枚举没有初始化, 即省掉"=整型常数"时, 则从第一个标识符开始。

```cpp
enum color { red, green, blue } c;
c = blue;//c 为枚举变量，把枚举元素 blue 赋给 c，此时 c = 2
// 若直接这样赋值 c = 2；就会报错！你只能把{ red, green, blue }赋值给 c。
// blue = 2；报错！blue 是常量而不是变量。
//未初始化默认red = 0,green = 1, blue = 2
```

若给某一个标识符赋值如 green = 5

```cpp
enum color { red, green = 5, blue } c;
c = blue;//6
//部分值初始化，要满足后面的值比前面值大 1，此时 red 默认为 0
//green 初始化为 5 ，blue 要满足比 green 大 1 即 blue = 6
```

### [C结构体、C++结构体、C++类的区别](https://www.cnblogs.com/cthon/p/9170596.html) 

**C结构体与C++结构体**

- C语言中的结构体不能为空，否则会报错
- C语言中的结构体只涉及到数据结构，而不涉及到算法，也就是说在C中数据结构和算法是分离的。换句话说就是C语言中的结构体只能定义成员变量，但是不能定义成员函数（虽然可以定义函数指针，但毕竟是指针而不是函数）。然而C++中结构体既可以定义成员变量又可以定义成员函数， C++中的结构体和类体现了数据结构和算法的结合。

**C++中结构体与类**

- 相同之处： 结构体中也可以包含函数；也可以定义public、private、protected数据成员；定义了结构体之后，可以用结构体名来创建对象。也就是说在C++当中，结构体中可以有成员变量，可以有成员函数，可以从别的类继承，也可以被别的类继承，可以有虚函数。总的一句话：class和struct的语法基本相同，从声明到使用，都很相似，但是struct的约束要比class多，理论上，struct能做到的class都能做到，但class能做到的stuct却不一定做的到。
- 区别：对于成员访问权限和继承方式，class中默认的是private，而struct中则是public。class还可以用于表示模板类型，struct则不行。

### 结构体与联合体（共用体）的区别

- 结构和联合都是由多个不同的数据类型成员组成, 但在任何同一时刻, 联合中只存放了一个被选中的成员（所有成员共用一块地址空间）, 而结构体的所有成员都存在（不同成员的存放地址不同）。 （在struct中，各成员都占有自己的内存空间，它们是同时存在的。一个struct变量的总长度等于所有成员长度之和。在Union中，所有成员不能同时占用它的内存空间，它们不能同时存在。Union变量的长度等于最长的成员的长度。）

- 对于联合的不同成员赋值, 将会对其它成员重写, 原来成员的值就不存在了, 而对于结构的不同成员赋值是互不影响的。
[C/C++结构体和联合体的区别](https://blog.csdn.net/dreamback1987/article/details/8504943)
