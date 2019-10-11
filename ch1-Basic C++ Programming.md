[TOC]

## C++编程基础

### Hello, world!
```cpp
#include <iostream>
using namespace std;
int main(int argc, char* argv[]) {
	cout << "Hello, world!" << endl;
	system("pause");
	return 0;
}
```
#### main 函数前面的数据类型 int 与 void
main 函数的返回值是返回给主调进程，使主调进程得知被调用程序的运行结果。

标准规范中规定 main 函数的返回值为 int，一般约定返回 0 值时代表程序运行无错误，其它值均为错误号，但该约定并非强制。

如果程序的运行结果不需要返回给主调进程，或程序开发人员确认该状态并不重要，比如所有出错信息均在程序中有明确提示的情况下，可以不写 main 函数的返回值。在一些检查不是很严格的编译器中，比如 VC, VS 等，void 类型的 main 是允许的。不过在一些检查严格的编译器下，比如 g++, 则要求 main 函数的返回值必须为 int 型。

所以在编程时，区分程序运行结果并以 int 型返回，是一个良好的编程习惯。

#### "\n" 与 endl 的区别

在 C++ 中，终端输出换行时，用 cout<<......<<endl 与 "\n" 都可以，这是初级的认识。但二者有小小的区别，用 endl 时会刷新缓冲区，使得栈中的东西刷新一次，但用 "\n" 不会刷新，它只会换行，栈内数据没有变化。但一般情况，二者的区别是很小的，建议用 endl 来换行。  
```cpp
std::cout << std::endl;
```
相当于:
```cpp 
std::cout << '\n' << std::flush;
```
或者
```cpp
std::cout << '\n'; std::fflush(stdout);
```

endl 除了写 '\n' 之外，还调用 flush 函数，刷新缓冲区，把缓冲区里的数据写入文件或屏幕。考虑效率就用 '\n'。

一般情况下，不加endl大多数情况下，也能正常输出，是因为在系统较为空闲时候，会查看缓存区的内容，如果发现新的内容，便进行输出。但是你并不清楚，系统什么时候输出，什么时候不输出，与系统自身的运行状况有关。而刷新缓存区，是强制性的，绝对性的输出，不取决于系统运行状况。所以正如《C++ Primer》书中所写，为了避免出现没有刷新输出流的情况发生，在使用打印语句来调试程序时，一定要加入 endl或 flush 操纵符。

这里可能会想到，以后遇到这类问题，干脆直接都使用 endl，不用 \n 不就好了吗？

也不是，要知道，endl会不停地刷新输出流，频繁的操作会降低程序的运行效率，这也是C++标准库对流的输入/输出操作使用缓冲区的原因。没有必要刷新输出流的时候应尽量使用 \n，比如对于无缓冲的流 cerr，就可以直接使用 \n。

#### 头文件里的 " " 与 < >
<> 先去系统目录中找头文件，如果没有在到当前目录下找。所以像标准的 C 头文件 stdio.h、stdlib.h 和 C++ 头文件 iostream、string 等用这个方法。
" " 首先在当前目录下寻找，如果找不到，再到系统目录中寻找。 这个用于 include 自定义的头文件，让系统优先使用当前目录中定义的。 

#### 命名空间 std

所谓名称空间它是一种将库函数封装起来的方法。通过这种方法，可以避免和应用程序发生命名冲突的问题。

如果想要使用 `cin`，`cout` 这两个 `iostream`对象，不仅要包含 `<iostream>` 头文件，还得让命名空间`std`内的名称曝光，即 `using namespace std;`

真正的开发过程中， 尽量避免使用 `using namespace std;` 等直接引入整个命名空间，否则会因为命名空间污染导致很多不必要的问题， 比如自己写的某个函数，名称正好和 std 中的一样， 编译器会不知道使用哪一个， 引起编译报错， 建议使用由命名空间组合起来的全称：
`std::cout << "Hello World" << std::endl;`

【ESC 6】

#### system("pause")
包含头文件 stdlib.h，并在主程序中加入 `system("pause");` 可以在程序运行完以后使黑框暂停显示，等待输入，而不是闪退。

#### cout 与 printf()
cout 流速度较慢，如果速度过慢可以用 <stdio.h> 库中的 printf() 格式化输出函数，不需要 `using namespace std;`。但注意 printf() 中不能使用 endl。printf 是函数。cout是ostream对象，和 << 配合使用。如果 printf 碰到不认识的类型就没办法了，而cout可以重载进行扩展。

#### main函数与命令行参数

一个程序的main()函数可以包括两个参数

- 第一个参数的类型为int型；
- 第二个参数为字符串数组。

通常情况下，将第一个参数命名为argc，第二个参数为argv（当然参数名字可以换）。由于字符串数组有两种表达方式，因此，main函数存在两种书写方法：

```C
int main(int argc, char* argv[])//这里使用char* argv[]

int main(int argc, char** argv)//这里使用char ** argv
```

int argc：表示字符串的数量，操作系统会自动根据第二个参数传入数字，程序员不用管，只需要正确使用即可。若用户输入N个字符串，那么argc= N + 1；因为 argv[0] 为程序的路径。  

char* argv[]：字符串数组，即多个字符串。为用户输入的一系列字符串，字符串之间以空格间隔，形式为：str1 str2 str3
在linux下，若存在可执行文件a.out，则运行该程序命令为：

```shell
./a.out str1 str2 str3
```

#### 编译

安装 g++

```shell
sudo apt-get install g++ build-essential
```

将源文件hello.cpp编译成可执行文件hello
```shell
g++ hello.cpp -o hello
```
运行可执行文件
```shell
./hello
```
如果省略`-o hello`也是没问题的。由于命令行中未指定可执行程序的文件名，编译器采用默认的 a.out。程序可以这样来运行：
```shell
./a.out
```

如果是多个 C++代码文件，如f1.cpp，f2.cpp，编译命令如下：
```shell
g++ f1.cpp f2.cpp -o myexec
```
则会生成一个名为`myexec`的可执行文件。

###  ### \<iostream\>

| 头文件      | 函数和描述                                                   |
| ----------- | ------------------------------------------------------------ |
| \<iostream> | 该文件定义了 cin、cout、cerr 和 clog 对象，分别对应于标准输入流、标准输出流、非缓冲标准错误流和缓冲标准错误流。 |
| \<iomanip>  | 该文件通过所谓的参数化的流操纵器（比如 setw 和 setprecision），来声明对执行标准化 I/O 有用的服务。 |
| \<fstream>  | 该文件为用户控制的文件处理声明服务。                         |

#### 标准输入流 cin
```cpp
char name[50];
short age;
cout << "请输入您的名称与年龄： ";
cin >> name >> age;
//C++ 编译器根据要输入值的数据类型，选择合适的流提取运算符来提取值，并把它存储在给定的变量中。
//流提取运算符 >> 在一个语句中可以多次使用
```

#### 标准输出流 cout
```cpp
char str[] = "Hello C++";
cout << "Value of str is : " << str << endl;
```

#### 标准错误流 cerr
cerr 对象附属到标准错误设备，通常也是显示屏，但是 cerr 对象是非缓冲的，且每个流插入到 cerr 都会立即输出。
```cpp
char str[] = "Unable to read....";
cerr << "Error message : " << str << endl;
```

#### 标准日志流 clog
clog 对象附属到标准错误设备，通常也是显示屏，但是 clog 对象是缓冲的。这意味着每个流插入到 clog 都会先存储在缓冲在，直到缓冲填满或者缓冲区刷新时才会输出。
```cpp
char str[] = "Unable to read....";
clog << "Error message : " << str << endl;
```
[cout、cerr 与 clog 的区别](https://blog.csdn.net/garfield2005/article/details/7639833)

```cpp
#include <iostream>
using namespace std;
int main()
{
    cout << "cout" << endl;
    cerr << "cerr" << endl;
    return 0;
}
```
linux下命令行输入：
```shell
g++ main.cpp -o a
./a >> test.log
```
终端输出“cerr”，  
打开test.log，里面只有一行字符串“cout”。

- cout默认情况下是在终端显示器输出，cout流在内存中开辟了一个缓冲区，用来存放流中的数据，当向cout流插入一个endl，不论缓冲区是否满了，都立即输出流中所有数据，然后插入一个换行符。cout可以被重定向到文件。

- cerr不经过缓冲而直接输出，一般用于迅速输出出错信息，是标准错误，默认情况下被关联到标准输出流，但它不被缓冲，也就说错误消息可以直接发送到显示器，而无需等到缓冲区或者新的换行符时，才被显示。不被重定向。
    - 有时程序调用导致栈被用完了，此时如果使用cout 会导致无内存输出。使用cerr 会在任何情况下输出错误信息。
    - 不被缓冲就是你打一个字符就马上在显示器显示，而不是等到endl才打印。

- clog流也是标准错误流，作用和cerr一样，区别在于cerr不经过缓冲区，直接向显示器输出信息，而clog中的信息存放在缓冲区，缓冲区满或者遇到endl时才输出。clog用的少。

### 初始化

```C++
int a(5);  // 构造函数语法，比如复数需要初始化两个值
complex<double> purei(0, 7);
int b = 6;  // C 风格的运算符（=）初始化

//关于指针
int arr[5] = {1, 2, 3, 4, 5};
int *p = arr; // 合法
cout << p[1] << endl;

vector<int> vec(arr, arr + 5);
vector<int> *pv = &vec;  // 合法
//而 vector<int> *pv = vec;  不合法，因为 vec 是 vector 型对象，它的名字不是首地址
cout << (*vec)[1] << endl;  // 注意与数组的差别
```

[关于C++中vector数组的首地址问题](https://blog.csdn.net/net_syc/article/details/79629842)

### 逗号表达式

```C++
cout << a_string << (cnt % line_size ? ' ' : '\n');
//若某一行计数不超过 line_size，就打印空格，否则换行。用来限制某一行字符打印的个数
//【ESC 11】
```

### 指针

如果指针不指向任何对象，则【提领】操作（也可称为【解引用】，即取指针指向的内容）会导致未知的执行结果。这意味着在使用指针时，必须在提领前确定它的确指向某对象。

一个未指向任何对象的指针，其地址为0，也被称为 null 指针。我们可以在定义阶段便初始化指针，令其值为 0.
```C++
int double *p = 0;  //初始化指针地址为0
if (p && *p != 1024)  //如果 p 地址不为零，才能有 *p 操作
    *p = 1024;
//当 p 的地址为 0，直接提领，会报错
//【ESC 28】
```

#### 数组指针与指针数组
```C
type (*p)[]; //数组指针
/*
type a[]，a 是一个数组，数组内元素都是 type 类型，将 a 换成 (*p)，
可以理解为 （*p）是一个数组，数组内元素都是 type 类型，那么 p 就是指向这样的数组的指针，即数组指针。
*/

type *p[]; //指针数组
/*
type *p[]即(type *)p[]，则 p 是一个 type* 类型的数组(类比int a[10]，a 是 int 型数组）
即 p 是一个数组，数组内元素都是指针（ type *）
*/
```

稍微复杂一些的指针数组：

```C++
const int seq_cnt = 6;
vector<int> *seq_addrs[seq_cnt] = {
    &fibonacci, &lucas, &pell,
    &triangular, &square, &pentagonal
};
//seq_addrs 是一个数组，其类型为 vector<int> *，
//seq_addrs[0] 的内容为指针，该指针指向 fibonacci，
//而 fibonacci 的类型为 vector<int>。
//【ESC 29】
```

#### 指针函数

首先这是一个函数，函数的返回值是指针。如

```C
char *StrCat(char *ptr1, char *ptr2)
{
    char *p;
    do something;
    return p;
}
```

#### 函数指针

首先这是一个指针，该指针指向函数。如

```C
#include <stdio.h>
int max(int x, int y)
{
    return x > y? x: y;
}
int main()
{
    //int max(int x, int y);
    int (*ptr)(int, int);
    ptr = max;
    int max_num = (*ptr)(12, 18);  //int max_num = ptr(12, 18);也是对的
    printf("%d", max_num);
    return 0;
}
//打印 18。
```

函数指针解引用与不解引用没有区别  
[函数指针解引用](https://bbs.csdn.net/topics/380152633)

```C
int (*ptr)();//“()”表明该指针指向函数，函数返回值为 int
int (*ptr)[3];//“[]”表明该指针指向一维数组，该数组里包含三个元素，每一个元素都是 int类型。
```

数组名指向了内存中一段连续的存储区域，可以通过数组名的指针形式去访问，也可以定义一个相同类型的指针变量指向这段内存的起始地址，从而通过指针变量去引用数组元素。

每一个函数占用一段内存区域，而函数名就是指向函数所占内存区的起始地址的函数指针（地址常量）。通过引用函数名这个函数指针让正在运行的程序转向该入口地址执行函数的函数体，也可以把函数的入口地址赋给一个指针变量，使该指针变量指向该函数。

#### 函数指针数组

```C++
const vector<int>* (*seq_array[])(int) = {
    fibon_seq, lucas_seq, pell_seq,
    triang_seq, square_seq, pent_seq
};
```

seq_array 是一个可以持有六个函数指针的指针数组，第一个元素指向函数 fibon_seq()，该函数原型为 `const vector<int> *fibon_seq(int);`。

【ESC 62】

### 指针与数组的区别

- 修改内容上的差别：

  ```C++
  char a[] = "hello";
  a[0] = 'H';
  char *p = "world"; // p 指向常量字符串，该字符串存储在文字常量区，不可更改
  //p[0] = "W"  // 所以这个赋值有问题
  ```

  ```C++
  int main() {
      char a[] = "hello";
      char *p = a;  // 这样让指针指向数组 a，而非常量字符串，就可以修改了
      p[0] = 'H';
      a[1] = 'E';
      printf("%d", sizeof(p));
      std::cout << p << std::endl; 
      return 0;
  }
  //输出：8HEllo 
  ```

- sizeof

  ```C++
  sizeof(a);  // 输出6，包含 '\0'
  sizeof(p);  //  64 位机器输出 8
  /*
  指针类型对象占用内存为一个机器字的长度：
  这意味着在16位cpu上是16位 = 2字节
  32位cpu上是32位 = 4字节
  64位上就是64位也就是8字节了
  */
  ```

### 指针与引用的区别

这个详细版，简略的回答可看【ESC 46，47】

- 指针是一个变量，只不过这个变量存储的是一个地址，这个地址指向内存的一个存储单元；而引用仅是个别名；

- 引用使用时无需解引用(*)，指针需要解引用；

-  引用只能在定义时被初始化一次，之后不可变；指针可变；

- 引用没有 const，指针有 const；

- 引用不能为空，指针可以为空；

- “sizeof 引用”得到的是所指向的变量(对象)的大小，而“sizeof 指针”得到的是指针本身的大小；

- 指针可以有多级，但是引用只能是一级（int **p；合法 而 int &&a是不合法的）

- 从内存分配上看：程序为指针变量分配内存区域，而引用不需要分配内存区域。

- 指针和引用的自增(++)运算意义不一样；

  ```C++
  int a = 0;
  int b = &a;
  int *p = &a;
  b++; // 相当于a++; b 只是 a 的一个别名，和 a 一样使用。
  p++; // p 指向 a 后面的内存
  (*p)++; // 相当于a++ 
  ```

### 指针的一些细节

#### \*&p和&\*p

- &:取出变量的存储地址;对指针变量p,&p取指针变量p所占用内存的地址，可以说是二级指针。  
- \*:引用指针所指向单元的内容。

```C++
1、*&p 等价于*(&p)。
2、&*p 等价于&(*p)。
p 是 int 变量，那么 *&p = p，而 &*p 是非法的。因为 *p 非法。
p 是 int * 指针变量，那么 *&p = p，&*p = p，都是 p。
```

#### p+(或-)n

指针加减一个数，是指该指针上移或下移n个数据之后的内存地址。即：

```C++
p +(或-) n * sizeof(type);
```

#### \*p++， (\*p)++，++\*p，\*++p

- \*p++ 和 \*(p++) 没有区别，应该理解为，由于后++优先级高于\*，应该先 p++，后取值，但因为 ++ 在变量之后，运算得先用再自增，所以先执行\*p，p再自增。这又与 (\*p)++ 有区别，这里面先 \*p，\*p 再自增而不是 p 自增。

  ```C++
  //一个++在变量后，先用再自增的例子。
  int a = 3;
  int b = 3;
  printf("%d",a++); //打印3,但a值已经变成4了。先用a，a再自增，所以就是先打印3,打印完再自增。
  printf("%d",++b); //先自增，自增完，再用。
  ```

- \*++p 等价于 \*(++p)，p 先自增，自增完后再取 \*p 的值，即取下一个元素的值，而不是当前元素。    
  ++\*p，*p自增。  

