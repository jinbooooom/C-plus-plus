## C++编程基础

### 名称空间

所谓名称空间它是一种将库函数封装起来的方法。通过这种方法，可以避免和应用程序发生命名冲突的问题。

如果想要使用 `cin`，`cout` 这两个 `iostream`对象，不仅要包含 `<iostream>` 头文件，还得让命名空间`std`内的名称曝光，即 `using namespace std;`

【ESC 6】

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

