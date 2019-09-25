## 类 & 对象

包含对象所需的数据，以及描述用户与数据交互所需的操作。

成员函数可以定义在类定义内部，或者单独使用**范围解析运算符 ::** 来定义。在类定义中定义的成员函数把函数声明为**内联**的，即便没有使用 inline 标识符。

### 类访问修饰符

- public：公有成员在程序中类的外部是可访问的。可以不使用任何成员函数来设置和获取公有变量的值。
- private：私有成员变量或函数在类的外部是不可访问的，甚至是不可查看的。只有类和友元函数可以访问私有成员。

- protected：保护成员变量或函数与私有成员十分相似，但有一点不同，保护成员在派生类（即子类）中是可访问的。

### 继承

- private 成员只能被本类成员（类内）和友元访问，不能被派生类访问；

- protected 成员可以被派生类访问。

### 构造函数与析构函数
```C++
class Line
{
	private:
	double length;
	
	public:
		void setLength(double len);
		Line(double len); //构造函数的名称与类的名称完全相同。不会返回任何类型，也不会返回void，常用于赋初值
		~Line();  //析构函数，函数名与类完全相同，只是在前面加了一个波浪号（~）作为前缀，它不会返回任何值，也不会返回void，也不能带有任何参数。析构函数有助于在跳出程序（比如关闭文件、释放内存等）前释放资源。
		void Line::getLength(void)  //方法可以定义在类中
        {
			return length;
		}
}

Line::Line(double len) //方法也可以通过范围解析运算符定义在类外，这是构造函数的具体实现，注意函数前无类型
{
	cout << "Object is being created, length = " << len << endl;
	length = len; //初始化属性
}

Line::~Line(void)
{
	cout << "Object is being deleted" << endl;
}

```
也可以使用初始化列表来初始属性
```C++
Line::Line( double len): length(len)
{
    cout << "Object is being created, length = " << len << endl;
}

```
```C++
// 同时初始化多个值
Person::Person( double name, double age, double job): Name(name), Age(age), Job(job)  
//将参数 name, age, job 初始化给属性 Name, Age, Job
{
  ....
}
```
### 拷贝构造函数
```C++
class Line
{
    public:
    	Line(int len);  //构造函数
    	Line(const Line &obj);  //拷贝构造函数
    	~Line();  //  析构函数
    
    private:
    	int *ptr;
}

// 成员函数定义，包括构造函数
Line::Line(int len)
{
    cout << "调用构造函数" << endl;
    // 为指针分配内存
    ptr = new int;
    *ptr = len;
}
 
Line::Line(const Line &obj)
{
    cout << "调用拷贝构造函数并为指针 ptr 分配内存" << endl;
    ptr = new int;
    *ptr = *obj.ptr; // 拷贝值
}
 
Line::~Line(void)
{
    cout << "释放内存" << endl;
    delete ptr;
}
```
#### 必须定义拷贝构造函数的情况：
只包含类类型成员或内置类型（但不是指针类型）成员的类，无须显式地定义拷贝构造函数也可以拷贝；有的类有一个数据成员是指针，或者是有成员表示在构造函数中分配的其他资源，这两种情况下都必须定义拷贝构造函数。

#### 什么情况使用拷贝构造函数：
类的对象需要拷贝时，拷贝构造函数将会被调用。以下情况都会调用拷贝构造函数：
- 通过使用另一个同类型的对象来初始化新创建的对象。
- 复制对象把它作为参数传递给函数。
- 复制对象，并从函数返回这个对象。

### 友元函数
类的友元函数是定义在类外部，但有权访问类的所有私有（private）成员和保护（protected）成员。尽管友元函数的原型有在类的定义中出现过，但是友元函数并不是成员函数。  
友元可以是一个函数，该函数被称为友元函数；友元也可以是一个类，该类被称为友元类，在这种情况下，整个类及其所有成员都是友元。
```C++
class Box
{
	double width;
	public:
		friend void printWidth( Box box );  // printwidth 能够调用 Box类的所有私有（private）成员和保护（protected）成员
		void setWidth( double wid );
};
void printWidth( Box box )  // printWidth() 不是任何类的成员函数
{
   /* 因为 printWidth() 是 Box 的友元，它可以直接访问该类的任何成员 */
   cout << "Width of box : " << box.width <<endl;
}
```

### 内联函数

C++ 内联函数是通常与类一起使用。如果一个函数是内联的，那么在编译时，编译器会把该函数的代码副本放置在每个调用该函数的地方。在类中定义的函数都是内联函数，即使不用 inline 说明符。
引入内联函数的目的是为了解决程序中函数调用的效率问题，因为编译器使用相同的函数代码代替函数调用，对于内联代码，程序无需跳转到另一个位置执行代码，再跳回来。因此内联函数的运行速度比常规函数稍快，但代价是需要占用更多的内存。如果在程序的多个不同的地方调用内联函数，该程序将包含该内联函数的多个副本。总的来说就是用空间换时间。所以内联函数一般都是1-5行的小函数。关于内联函数可以总结为：

- 相当于把内联函数里面的内容写在调用内联函数处；
- 相当于不用执行进入函数的步骤，直接执行函数体；
- 相当于宏，却比宏多了类型检查，真正具有函数特性；
- 不能包含循环、递归、switch 等复杂操作；
- 在类声明中定义的函数，除了虚函数的其他函数都会自动隐式地当成内联函数。在类外定义需要显式内联
使用内联函数不过是向编译器提出一种申请，编译器可以拒绝你的申请。
