### 输入输出

```C++
# define SIZE 20;
char s[SIZE];
cin>>s;   
/*由于不能用键盘输入‘\0’，所以cin不是用空字符来作为字符串的结尾位置.cin使用空白空格制表符和换行符来作为字符的结束位置。这意味着一段字符串带空格，cin只会读取空格前面那一部分。*/

cin.getline(s, SIZE)； 
/*读取整行，以输入回车键键入的换行符作为输入结尾。使用cin.getline()最多能保存SIZE-1长度的字符，储存完s后，丢弃换行符，多余的空间全部用空字符填充*/

cin.get(s, SIZE)；
/*读入输入队列在换行符前面的那部分，不同于cin.getline()，cin.get(s, SIZE)将换行符留在输入队列里，这样会导致在第二次调用时，看到的第一个字符就是留下的换行符，因此cin.get(s, SIZE)认为到达行尾，没有什么内容可以读取。此时应该用不带参数的cin.get()读取下一个字符*/
// [CPP78,80]

string s1, s2;
getline(cin, s1);
getline(cin, s2);
cout << s1 << endl << s2 << endl;
/*对 getline() 会将换行之前的所有字符都读到string类型变量中，而不会被空格隔开*/
```

### 字符串

- [string](https://blog.csdn.net/qq_30534935/article/details/82227364)

#### C++ 风格

```C++
#include <string>
string s1, s2, s3;
s2 = s1;  //赋值
s3 = s1 + s2; //拼接字符串 s1, s2
s1.size(); //或者 s1.length()，返回字符串长度
s1.append(s); //末尾添加
s1.find(s); //返回s在s1中第一次出现的索引（int），若没有出现，返回 -1 
s1.replace(p, n, s);//替换字符串，例如string s1 = "abcdefggh"; s1.replace(3, 2, "z");输出s1:"abczfggh"
    
```

#### C 风格：

```C++
#include <cstring>
char s1[20], s2[20];
strcpy(s1, s2); //赋值s2到s1（s1被取代）
strcat(s1, s2); //将s2添加到s1的末尾
strlen(s1); //返回s1的长度，不可接受string对象，只能接受C风格
strcmp(s1,s2); //若s1与s2相同，返回0,若s1<s2则返回值小于零，s1>s2返回值大于零
strchr(s1, ch); //返回一个char*指针，指向字符串s1中字符ch第一次出现的位置
strstr(s1, s2); //返回一个指针，指向字符串s1中s2第一次出现的位置

```

