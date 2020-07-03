# CPP学习笔记

------

# 2020.7.20

## 为什么要防止头文件被重复包含:

头文件被重复包含会导致变量的重复定义，进而导致编译器的困惑，最后报错。

## extern C的意义：

cpp支持函数重载，而c不支持函数重载。因而编译器有所不同，为了让cpp工程中正确编译c函数，须要加上extern C。

# 2020.6.30

## int& 与 &：

- int& 指的是引用，不是取地址
- 单独&则是取地址

## #define与const区别：

- 编译处理阶段不同：define在预处理阶段进行，const是在编译运行阶段。
- 类型：const常量有具体的类型，而define没有。
- 安全检查：const在编译阶段会执行类型检查，而define不会进行检查。
- 存储方式不同：const常量会进行内存分配，而define仅仅是展开，不会分配内存。

## struct、class区别：

- 默认访问权限：struct默认访问权限是pubulic，class是private
- 默认继承权限：struct默认继承权限是pubulic，class是private
- 定义函数：struct里不可以定义函数，class可以

## java、cpp的区别：

- 【语言特性】Java是完全面向对象的，所有方法都必须写在类中。cpp可以面向过程。
- 【垃圾回收机制】java因为是运行在虚拟机上，不需要考虑内存管理和垃圾回收机制。也是就你可以声明一个对象而不用考虑释放他，虚拟机帮你做这事情。写c和c++程序如果用到指针就一定要考虑内存申请和释放。内存泄漏是c和c++最头疼的问题。
- 【应用场景】
  - cpp：开发桌面软件、操作系统、图形处理、游戏
  - 移动（Android）应用

## 引用、指针的区别

【相关知识】函数的参数传递或者返回值有三种传递方式：值传递、引用传递、指针传递。

【概念】引用是对象的别名，对象还是那个对象，只不过名字变了；指针指向对象的地址。

【区别】

- 内存的区别：指针需要分配新的内存、引用不需要
- 自增的区别：指针自增是指向下一个地址，引用自增变量值+1

## new、malloc区别：

- 内存分配：new的内存开辟于自己存储区、malloc内存开辟在堆区；new无需指明内存块大小，malloc需指明内存大小
- 语言：new/delete只能在c++使用，malloc/free在c和cpp中都可以使用
- 析构函数：new/delete会自动调用析构函数，而malloc/free不会自动调用析构函数
- 返回类型：new的返回类型为对象类型、malloc返回void*

# 2020.5.25

## C中的可变参数：

```C
#include <stdio.h>
#include <stdarg.h>
 
double average(int num,...)
{
    va_list valist;
    double sum = 0.0;
    int i;
 
    /* 为 num 个参数初始化 valist */
    va_start(valist, num);
 
    /* 访问所有赋给 valist 的参数 */
    for (i = 0; i < num; i++)
    {
       sum += va_arg(valist, int);
    }
    /* 清理为 valist 保留的内存 */
    va_end(valist);
 
    return sum/num;
}
```

# 2019.2.21
## functor
functor就是一个重载了 operator()的类，因为重载了(),所以用这个类生成的实例就像一个函数。（functor就是一个作为函数用的类）

仔细体会以下代码
```cpp
class StringAppend{
    public:
        explicit StringAppend(const string& str) : ss(str){}

        void operator() (const string& str) const{
             cout<<str<<' '<<ss<<endl;
        }
    
    private:
        const string ss;  
};

StringAppend myFunc("is world");
myFunc("hello");
>>>hello is world
```

简单应用例子应用场景如下：
```cpp
class ShorterThan {
    public:
        explicit ShorterThan(int maxLength) : length(maxLength) {}
        bool operator() (const string& str) const {
            return str.length() < length;
        }
    private:
        const int length;
};

count_if(myVector.begin(), myVector.end(), ShorterThan(length));//直接调用即可
```
在SLAM中的非线性优化应用场景如下：
```cpp
#include <iostream>
#include <opencv2/core/core.hpp>
#include <ceres/ceres.h>
#include <chrono>

using namespace std;

// 代价函数的计算模型,也即functor
struct CURVE_FITTING_COST
{
    CURVE_FITTING_COST ( double x, double y ) : _x ( x ), _y ( y ) {}
    // 残差的计算
    template <typename T>
    bool operator() (
        const T* const abc,     // 模型参数，有3维
        T* residual ) const     // 残差
    {
        residual[0] = T ( _y ) - ceres::exp ( abc[0]*T ( _x ) *T ( _x ) + abc[1]*T ( _x ) + abc[2] ); // y-exp(ax^2+bx+c)
        return true;
    }
    const double _x, _y;    // x,y数据
};

int main ( int argc, char** argv )
{
    double a=1.0, b=2.0, c=1.0;         // 真实参数值
    int N=100;                          // 数据点
    double w_sigma=1.0;                 // 噪声Sigma值
    cv::RNG rng;                        // OpenCV随机数产生器
    double abc[3] = {0,0,0};            // abc参数的估计值

    vector<double> x_data, y_data;      // 数据

    cout<<"generating data: "<<endl;
    for ( int i=0; i<N; i++ )
    {
        double x = i/100.0;
        x_data.push_back ( x );
        y_data.push_back (
            exp ( a*x*x + b*x + c ) + rng.gaussian ( w_sigma )
        );
        cout<<x_data[i]<<" "<<y_data[i]<<endl;
    }

    // 构建最小二乘问题
    ceres::Problem problem;
    for ( int i=0; i<N; i++ )
    {
        problem.AddResidualBlock (     // 向问题中添加误差项
        // 使用自动求导，模板参数：误差类型，输出维度，输入维度，维数要与前面struct中一致
            new ceres::AutoDiffCostFunction<CURVE_FITTING_COST, 1, 3> ( 
                new CURVE_FITTING_COST ( x_data[i], y_data[i] )
            ),
            nullptr,            // 核函数，这里不使用，为空
            abc                 // 待估计参数
        );
    }

    //...........
}


```
如果不理解，详细说明可以在[博客园这里](https://www.cnblogs.com/decade-dnbc66/p/5347088.html)找到。


# 2016.8.25
## 递归函数

```cpp
/*递归函数应用于计算阶乘*/
int factorial(int a)
{
	if (a == 1)
		return 1;
	return(a*factorial(a - 1));
}
/*递归函数应用于计算斐波那契数列*/
int Fibonacci(int n)
{
    if (n <= 1)  
        return n;  
    else  
        return Fibonacci(n-1) + Fibonacci(n-2);  
}
//递归函数应用于->用位运算实现两位数的加法
int add(int a, int b)
{
	if (0 == b)
		return a;
	int sum, carry;
	sum = a^b;//没有发生进位的位数
	carry = (a&b) << 1;//取出有发生进位的位数，再进行左移，相当于进位
	return add(sum, carry);
}
```

**@用结构体说明链表的示例**

```cpp
struct Student
{
	int num;
	float score;
	struct Student *pNext;
}student1, student2, student3;

void main()
{
	struct Student *head, *p;
	student1.num = 2013211;
	student1.score = 90;
	student2.num = 2013212;
	student2.score = 95;
	student3.num = 2013213;
	student3.score = 100;
	head = &student1;//把学生1放在链表头
	student1.pNext = &student2;
	student2.pNext = &student3;
	student3.pNext = NULL;
	p = head;//p也指向链表头

	while (p != NULL)
	{
		printf("%d\n  %4.1f \n    ", p->num, p->score);
		p = p->pNext;
	}
}
```

**@new创建对象 与 不new的区别**

1.new创建类对象使用完需delete销毁
2.new创建对象直接使用**堆空间**，而局部不用new定义类对象则使用**栈空间**
3.new创建类对象需要**指针接收**，一处初始化，多处使用

# 2016.8.23

## 指向成员变量的指针

```
//一直写错！！！！搞浑！！！！！！！！
int Car::*pSpeed = &Car::speed;//定义一个指向成员变量speed的指针
int (Car::*pGetSpeed)() const= &(Car::GetSpeed);//定义一个指向成员函数的指针
```

```cpp
#include<iostream>
using namespace std;

class Car
{
public:
	int speed;
};

int main()
{
	int Car::*pSpeed = &Car::speed;//定义一个指向成员变量speed的指针

	Car c1;
	c1.speed = 1;       // direct access
	cout << "speed is " << c1.speed << endl;
	c1.*pSpeed = 2;     // access via pointer to member
	cout << "speed is " << c1.speed << endl;
	return 0;
}
```

## 指向成员函数的指针

```cpp
#include<iostream>
using namespace std;

class Car
{
public:
	void SetSpeed(int _speed)
	{
		speed = _speed;
	}
	int GetSpeed()  const
	{
		return speed;
	}
private:
	int speed;
};

int main()
{
	int (Car::*pGetSpeed)() const= &(Car::GetSpeed);
	Car car;
	car.SetSpeed(10);
	cout << (car.*pGetSpeed)() << endl;//注意 (car.*pGetSpeed)()的写法而不是car.*pGetSpeed()！！！！
	return 0;
}

```

# 2016.8.22
## 一个有意思，值得学习的C++程序

```cpp
#include <iostream>

class bowl {
public:
	int apples;
	int oranges;
};

/*int bowl::*fruit 为定义一个指向成员变量的指针*/
int count_fruit(bowl * begin, bowl * end, int bowl::*fruit)
{
	int count = 0;
	for (bowl * iterator = begin; iterator != end; ++iterator)
		count += iterator->*fruit;
	return count;
}
int main()
{
	/*调用两次构造函数*/
	bowl bowls[2] = {
		{ 2,4},
		{ 3, 5}
	};
	
	/*注意两个函数中调用参数方法的不同*/
	std::cout << "I have " << count_fruit(bowls, bowls + 2, &bowl::apples) << " apples\n";
	std::cout << "I have " << count_fruit(&bowls[0], &bowls[0] + 2, &bowl::oranges) << " oranges\n";
	return 0;
}
```

# 2016.8.17

## 回调函数

概念：回调函数，顾名思义，就是使用者自己定义一个函数，使用者自己实现这个函数的程序内容，然后把这个函数作为参数传入别人（或系统）的函数中，由别人（或系统）的函数在运行时来调用的函数。函数是你实现的，但由别人（或系统）的函数在运行时通过参数传递的方式调用，这就是所谓的回调函数。简单来说，就是由别人的函数运行期间来回调你实现的函数


```cpp
//定义带参回调函数
void PrintfText(char* s)
{
    printf(s);
}
//定义实现带参回调函数的"调用函数"
void CallPrintfText(void (*callfuct)(char*),char* s)
{
    callfuct(s);
}
//在main函数中实现带参的函数回调
int main(int argc,char* argv[])
{
    CallPrintfText(PrintfText,"Hello World!\n");
    return 0;
}
```

## 用typedef定义函数指针的形式

用typedef定义函数指针的形式如下：

typedef 返回类型(*新类型)(参数表)

```cpp
using namespace std;

typedef int (*PFactorialFunc)(int );  //宏定义FactorialFunc函数指针类型,
 
int  FactorialFunc(int a)              //计算a的阶乘
{
 int result=1;
 for (int i = 1; i <=a; i++)
 {
        result = result * i;
 }
 return result;
}

int main()
{
 PFactorialFunc pFactorialFunc;      //并定义一个指向某种函数的指针
 pFactorialFunc = FactorialFunc;    
 cout << (*pFactorialFunc）(4) << endl;
 return false;
}
```


因为受MFC的影响，在MFC中调试的时候都是直接按F5，或者本地”WINDOWS调试器“按钮。但在VS中的控制台程序中，按F5或者是”WINDOWS调试器“按钮，DOS界面都会一闪而过。

# 2016.8.16

## VS中控制台程序值得注意的地方

因为受MFC的影响，在MFC中调试的时候都是直接按F5，或者本地”WINDOWS调试器“按钮。但在VS中的控制台程序中，按F5或者是”WINDOWS调试器“按钮，DOS界面都会一闪而过。

解决方法：
1.按ctrl+F5(最直接的方法)
2.在程序末尾加入：int a;      cin>>a  (间接的方法，使要求输入一个数字)
#2016.8.12
**@*.dll**

DLL(Dynamic Link Library)文件为动态链接库文件，又称“应用程序拓展”。在Windows中，许多应用程序并不是一个完整的可执行文件，它们被分割成一些相对独立的动态链接库，即DLL文件，放置于系统中。当我们执行某一个程序时，相应的DLL文件就会被调用。一个应用程序可使用多个DLL文件，一个DLL文件也可能被不同的应用程序使用，这样的DLL文件被称为共享DLL文件。[1]

**@_stdcall以及相关函数调用引出的面试题**

常见的函数调用约定：stdcall         cdecl            fastcall          thiscall               naked call

函数调用约定主要约束了两件事：
1.参数传递顺序
2.调用堆栈由谁（调用函数或被调用函数）清理

stdcall: 
a)参数从右向左压入堆栈 
 b)函数被调用者修改堆栈

__cdecl：是C Declaration的缩写,表示C语言默认的函数调用方法：
a)所有参数从右到左依次入栈，
b)这些参数由调用者清除，称为手动清栈。

PASCAL 是Pascal语言的函数调用方式，也可以在C/C++中使用，
a)参数压栈顺序与前两者相反。（由左向右）
b)返回时的清栈方式与_stdcall相同。（被调用清理空间）


# 2016.8.11

## " "与 ‘ ’的区别

' '为字符
”“为字符串

'a'是一个字符
"a"是两个字符，'a'和'\0'

比如，下面这个例子：

char a1[1]={'a'};
char a2[1]={"a"};

前者a1[0]='a',能编译通过
后者编译通不过，因为a2是一个元素的数组，而"a"有两个元素，分别是'a'和'\0'

补充：
'aa'这是错误的写法，单引号是字符的引号，它只能引一个字符的
"aa"这是正确的写法，双引号是字符串的引号，它有三个字符：'a','a','\0'

# 2016.8.10
## 命令行参数：

C/C++语言中的 main 函数，经常带有参数 argc（argument count）， argv（argument vector）， ，如下：
int main(int argc, char** argv)
或者
int main(int argc, char* argv[])
在上面代码中， argc 表示命令行输入参数的个数（以空白符分隔）， argv 中
存储了所有的命令行参数。

## 关于结构体

例如定义了一个结构体
struct workerData
{
 int num;
 TCHAR name[20];
 float  wage;
};

要在一个函数中调用该结构体的变量


```cpp
void CWorkInfoDlg::Save(  )
{
    //直接workerData.num是不行的！！！！！！！！！！！
    //方法一
     workerData  data;
     data.num = _wtoi(pList->GetItemText(n,0));
     pList->GetItemText(n, 1, data.name, sizeof(data.name));//注意_countof()的形参为ARRAY,即数组！
     data.wage = _wtof(pList->GetItemText(n, 2));
 }
 
//方法二
void CWorkInfoDlg::Save( struct workerData data ) //在函数形参中定义
{
    //直接workerData.num是不行的！！！！！！！！！！！
     data.num = _wtoi(pList->GetItemText(n,0));
     pList->GetItemText(n, 1, data.name, sizeof(data.name));//注意_countof()的形参为ARRAY,即数组！
     data.wage = _wtof(pList->GetItemText(n, 2));
 }
 
//方法三
struct workerData
{
 int num;
 TCHAR name[20];
 float  wage;
}data;                       //在定义结构体的同时直接定义
```


# 2016.8.6

## 多态：

多态性的概念：向不同的对象发送同一个消息，不同的对象在接收时会产生不同的行为(即方法)
多态性分为两类: 静态多态性和动态多态性。以前学过的函数重载和运算符重载实现的多态性属于静态多态性，动态多态性是通过虚函数(virtual function)实现的。

## 虚函数：

1．虚函数的作用是:允许在派生类中重新定义与基类同名的函数，并且可以通过基类指针或引用来访问基类和派生类中的同名函数。


2．虚函数的使用方法：
(1) 在基类用virtual声明成员函数为虚函数。这样就可以在派生类中重新定义此函数，为它赋予新的功能，并能方便地被调用。在类外定义虚函数时，不必再加virtual。
(2) C++规定，当一个成员函数被声明为虚函数后，其派生类中的同名函数都自动成为虚函数。

3．什么情况下应当声明虚函数
(1) 首先看成员函数所在的类是否会作为基类。然后看成员函数在类的继承后有无可能被更改功能，如果希望更改其功能的，一般应该将它声明为虚函数。
(2) 如果成员函数在类被继承后功能不需修改，或派生类用不到该函数，则不要把它声明为虚函数。不要仅仅考虑到要作为基类而把类中的所有成员函数都声明为虚函数。


4.虚函数与重载的区别：
以前介绍的函数重载处理的是同一层次上的同名函数问题，而虚函数处理的是不同派生层次上的同名函数问题，前者是横向重载，后者可以理解为纵向重载。但与重载不同的是: 同一类族的虚函数的首部是相同的，而函数重载时函数的首部是不同的(参数个数或类型不同)。

##  纯虚函数：

作用：纯虚函数的作用是在基类中为其派生类保留一个函数的名字，以便派生类根据需要对它进行定义。如果在基类中没有保留函数名字，则无法实现多态性。

##  抽象类：

（1）定义：不用来定义对象而只作为一种基本类型用作继承的类，称为抽象类(abstract class)，
（2）凡是包含纯虚函数的类都是抽象类。因为纯虚函数是不能被调用的，包含纯虚函数的类是无法建立对象的。抽象类的作用是作为一个类族的共同基类，或者说，为一个类族提供一个公共接口，其中，**公共接口就是纯虚函数**。
（3）如果在派生类中没有对所有纯虚函数进行定义，则此派生类仍然是抽象类，不能用来定义对象。

 


# 2016.7.25

## C++中一些有困惑的程序

```cpp
class Sample
{
 int x, y;
public:
 Sample(int a,int b)
 {
  x = a;
  y = b;
 }
 int getx(){return x;}
 int gety(){return y;}
};
int main()
{
 int (Sample::*fp)();                //定义一个能指向成员函数的指针
 fp = &(Sample::getx);          //取成员函数getx()的地址赋给fp
 Sample a(1, 2);
 int v = (a.*fp)();         //（a.*fp）()  <==>   a.getx()
 cout << v << endl;
 return 0;
}
 
class Sample
{
 
public:
        int x, y;
 Sample(int a,int b)
 {
  x = a;
  y = b;
 }
 int getx(){return x;}
 int gety(){return y;}
};
int main()
{
 int Sample::*p;    //定义一个能指向成员变量的指针
 p = &Sample::y;    //取成员变量y的地址赋给p
 Sample b(3, 4);
 cout << b.*p << endl;     //  b.*p  <==>   b.y 
 return 0;
}
 
```

## explicit 关键字

作用：      explicit   只对构造函数起作用，用来抑制隐式转换。

什么是隐式转换？


```
class String
{
String ( const char* p ); // 用C风格的字符串p作为初始化值
//…
}
String s1 = “hello”; //OK 隐式转换，等价于String s1 = String（“hello”）;
//<==>  String temp("hello");  String s1=temp;
```

 

但是有的时候可能会不需要这种隐式转换，如下：
class String {
       String ( int n ); //本意是预先分配n个字节给字符串
String ( const char* p ); // 用C风格的字符串p作为初始化值
//…
}
下面两种写法比较正常：
String s2 ( 10 );   //OK 分配10个字节的空字符串
String s3 = String ( 10 ); //OK 分配10个字节的空字符串

下面两种写法就比较疑惑了：
String s4 = 10; //编译通过，也是分配10个字节的空字符串
String s5 = ‘a’; //编译通过，分配int（‘a’）个字节的空字符串

s4 和s5 分别把一个int型和char型，隐式转换成了分配若干字节的空字符串，容易令人误解。
为了避免这种错误的发生，我们可以声明显示的转换，使用explicit 关键字：
class String {
       explicit String ( int n ); //本意是预先分配n个字节给字符串
String ( const char* p ); // 用C风格的字符串p作为初始化值
//…
}
加上explicit，就抑制了String ( int n )的隐式转换，

# 2016.7.24

## 继承须要注意的：

 定义一个Myclass的类，Myclass *p  并不会调用Myclass 类的构造函数，只是定义一个可以指向Myclass类的对象.

# 2016.7.22

## 前增量与后增量（用于++i,i++的运算符重载）

前增量返回引用
后增量返回对象

使用前增量时,对对象(操作数)进行增量修改,然后再返回该对象.所以前增量运算符操作时,参数与返回的是同一个对象.着与基本数据类型的前增量操作相似,返回的也是左值.
使用后增量时,必须在增量之前返回原有的对象值.为此,需要创建一个临时对象,存放原有的对象,以便对操作数(对象)进行增量修改时,保存最初的值.后增量操作返回的是原有对象的值,不是原有对象,原有对象已被增量修改,所以返回的应该是存放原有对象值的临时对象.
C++约定，在增量运算符定义中，放上一个整数形参，就是后增量运算符。 

 

```cpp
// ++i  
A& operator++()  
{  
   ++m_i;  
   return *this;  
}  
// i++  
const A operator++(int)  
{  
   A tmp = *this;  
   ++(*this);    // Implemented by prefix increment  
   return A(tmp);  
}  
```

# 2016.6.10

## 单例模式的初步理解

http://blog.csdn.net/hackbuteer1/article/details/7460019

# 2016.6.4

## 递归函数

```c
long fact(int n)   
{   
  if(n==1)   
  return 1；   
  return fact(n-1)*n； //出现函数自调用   
} 
```

## 友元函数的简单了解：

友元函数和类的成员函数的区别：成员函数有this指针，而友元函数没有this指针。
记忆：A是B的友元《=》A是B的朋友《=》借助B的对象，在A中可以直接通过B。成员变量（可以是公有，也可以为私有变量）的方式访问B

# 2016.6.3

## const巧记：

记忆和理解的方法：
先忽略类型名(编译器解析的时候也是忽略类型名)，我们看const离哪个近。"近水楼台先得月"，离谁近就修饰谁。判断时忽略括号中的类型

```cpp
const (int) *p;   //const修饰*p，*p是指针指向的对象，不可变
(int) const *p；  //const修饰*p，*p是指针指向的对象，不可变
(int)*const p;   //const修饰p，p不可变，p指向的对象可变
const (int) *const p;  //前一个const修饰*p，后一个const修饰p，指针p和p指向的对象都不可变
```

## const修饰函数传入参数

​    将函数传入参数声明为const，以指明使用这种参数仅仅是为了效率的原因，而不是想让调用函数能够修改对象的值。同理，将指针参数声明为const，函数将不修改由这个参数所指的对象。
​    通常修饰指针参数和引用参数：
void Fun( const A *in); //修饰指针型传入参数
void Fun(const A &in); //修饰引用型传入参数

## const修饰成员函数(c++特性)

- 如果一个成员函数的不会修改数据成员，那么最好将其声明为const，因为const成员函数中不允许对数据成员进行修改，如果修改，编译器将报错，这大大提高了程序的健壮性。 一般放在函数体后，形如：void   fun()   const;  
- const对象只能访问const成员函数，而非const对象可以访问任意的成员函数，包括const成员函数；
-  const对象的成员是不能修改的，而通过指针维护的对象确实可以修改的；
-  const成员函数不可以修改对象的数据，不管对象是否具有const性质。编译时以是否修改成员数据为依据进行检查

## 顶层const与底层const

- 【概念】顶层const即指针本身不能改变；底层const即指针指向内容不能改变
- 【作用】避免不同类型的赋值问题，比如常量的底层const不能赋值给非常量的底层const

# 2016.6.2
## 交叉C的学习：数组与指针

int *p=&a[0]     等价于   int *p;  p=&a[0];    等价于    int *p=a;        (其中a为数组)

C语言中的规定：如果指针变量p已经指向一数组中的一个元素，则p+1指向同一数组中的下一个元素，而不是将p的值（地址）简单加1。

\*（p+5）等价于  \*(a+5)  等价于   a[5]
对数组元素a【i】就是按\*（a+i）处理的，【】实际上是变址运算符，即将a【i】按a+i 计算地址，然后找出该地址的值。

\*p++： ++和\*同优先级，结合方向自右而左          若p初值为a（即&a【0】），则\*（p++）为a【0】
                                                                                                                                 \*（++p）为a【1】

# 2016.5.31

## 拷贝构造函数与赋值运算符的重载

在面向对象程序设计中，对象间的相互拷贝和赋值是经常进行的操作。
    如果对象在申明的同时马上进行的初始化操作，则称之为拷贝运算。例如：
        class1 A("af"); class1 B=A;
         此时其实际调用的是B(A)这样的浅拷贝操作。
    如果对象在申明之后，再进行的赋值运算，我们称之为赋值运算。例如：
        class1 A("af"); class1 B;
        B=A;
        此时实际调用的类的缺省赋值函数B.operator=(A);

-----------------------------------------------------------------------------------

```cpp
A(A &a);//重载拷贝函数
A& operator=(A &b);//重载赋值函数  ，其中operator为C++中的关键字。
```

-----------------------------------------------------------------------------------

赋值运算符和拷贝函数很相似。只不过赋值函数最好有返回值（进行链式赋值），返回也最好是对象的引用（为什么不是对象本身呢？如果赋值运算符返回的是类对象本身，会调用类的拷贝构造函数，所以一定要overload 类的拷贝函数（进行深拷贝）！，如果赋值运算符返回的是对象引用，那么其不会调用类的拷贝构造函数，这是问题的关键所在！！）而拷贝函数不需要返回任何。同时，赋值函数首先要释放掉对象自身的堆空间（如果需要的话），然后进行其他的operation.而拷贝函数不需要如此，因为对象此时还没有分配堆空间。

------------原博文：http://www.cnblogs.com/winston/archive/2008/06/03/1212700.html--------------

# 2016.5.27
## 复制构造函数

以下三种情况拷贝构造函数将会被调用：
1)．一个对象以值传递的方式传入函数体        void display(A p) { ..... }           A a1(5,18) ;   display(a1);
2)．一个对象以值传递的方式从函数返回        A fun(A p){ ......   return p}            A a1(5,18);    A a2=fun(a1)
3)．一个对象需要通过另外一个对象进行初始化     A a1(5,18);      A a2(a1)

--------------以下文字能对复制构造函数有更好的理解-----------------
把参数传递给函数有三种方法，一种是值传递，一种是传地址，还有一种是传引用。前者与后两者不同的地方在于：当使用值传递的时候，会在函数里面生成传递参数的一个副本，这个副本的内容是按位从原始参数那里复制过来的，两者的内容是相同的。当原始参数是一个类的对象时，它也会产生一个对象的副本，不过在这里要注意。一般对象产生时都会触发构造函数的执行，但是在产生对象的副本时却不会这样，这时执行的是对象的复制构造函数。为什么会这样？嗯，一般的构造函数都是会完成一些成员属性初始化的工作，在对象传递给某一函数之前，对象的一些属性可能已经被改变了，如果在产生对象副本的时候再执行对象的构造函数，那么这个对象的属性又再恢复到原始状态，这并不是我们想要的。所以在产生对象副本的时候，构造函数不会被执行，被执行的是一个默认的构造函数。当函数执行完毕要返回的时候，对象副本会执行析构函数，如果你的析构函数是空的话，就不会发生什么问题，但一般的析构函数都是要完成一些清理工作，如释放指针所指向的内存空间。这时候问题就可能要出现了。假如你在构造函数里面为一个指针变量分配了内存，在析构函数里面释放分配给这个指针所指向的内存空间，那么在把对象传递给函数至函数结束返回这一过程会发生什么事情呢？首先有一个对象的副本产生了，这个副本也有一个指针，它和原始对象的指针是指向同块内存空间的。函数返回时，对象的析构函数被执行了，即释放了对象副本里面指针所指向的内存空间，但是这个内存空间对原始对象还是有用的啊，就程序本身而言，这是一个严重的错误。然而错误还没结束，当原始对象也被销毁的时候，析构函数再次执行，对同一块系统动态分配的内存空间释放两次是一个未知的操作，将会产生严重的错误。

拷贝构造函数是程序更加有效率，因为它不用再构造一个对象的时候改变构造函数的参数列表。

 ---------------------http://blog.csdn.net/feiyond/article/details/1807068-- 以上文章出处------------------

## 关于操作系统中的堆和栈的概念理解

预备知识-------------------程序的内存分配
一个由C/C++编译的程序占用的内存分为以下几个部分
1、栈区（stack）— 由编译器自动分配释放，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈
2、堆区（heap） — 一般由程序员分配释放，若程序员不释放，程序结束时可能由OS回收。注意它与数据结构中的堆是两回事，分配方式倒是类似于链表，呵呵。
3、全局区（静态区）（static）—，全局变量和静态变量的存储是放在一块的，初始化的全局变量和静态变量在一块区域，未初始化的全局变量和未初始化的静态变量在相邻的另一块区域。 - 程序结束后有系统释放
4、文字常量区 —常量字符串就是放在这里的。程序结束后由系统释放 
5、程序代码区—存放函数体的二进制代码。

经典程序例子：

```cpp
//main.cpp 
int a = 0; //全局初始化区
char *p1; //全局未初始化区
main()
{   
     int b;// 栈
     char s[] = "abc"; //栈
     char *p2; //栈
     char *p3 = "123456";// 123456/0在常量区，p3在栈上。
     static int c =0； //全局（静态）初始化区
     p1 = (char *)malloc(10);
     p2 = (char *)malloc(20);
```
     分配得来得10和20字节的区域就在堆区。
     strcpy(p1, "123456"); 123456/0放在常量区，编译器可能会将它与p3所指向的"123456"优化成一个地方。
}

## 关联到链表的知识

堆和栈申请后系统的响应
栈：只要栈的剩余空间大于所申请空间，系统将为程序提供内存，否则将报异常提示栈溢出。
堆：首先应该知道操作系统有一个记录空闲内存地址的链表，当系统收到程序的申请时，会遍历该链表，寻找第一个空间大于所申请空间的堆结点，然后将该结点从空闲结点链表中删除，并将该结点的空间分配给程序，另外，对于大多数系统，会在这块内存空间中的首地址处记录本次分配的大小，这样，代码中的delete语句才能正确的释放本内存空间。另外，由于找到的堆结点的大小不一定正好等于申请的大小，系统会自动的将多余的那部分重新放入空闲链表中。

---------------------http://blog.csdn.net/feiyond/article/details/1650698-- 以上文章出处------------------

 


# 2016.5.26
BSS段通常是指用来存放程序中未初始化的全局变量和静态变量的一块内存区域。特点是可读写的，在程序执行之前BSS段会自动清0。
可执行程序包括BSS段、数据段、代码段（也称文本段）。

# 2016.5.25
## static

- 【内存】应用程序所占据的内存可以分为静态存储区，栈区(stack)和堆区(heap)。在程序运行前就分配的存储空间一般都在静态存储区，静态变量分配的存储空间在栈区，动态内存分配的内存空间在堆区。
- 【对象】静态数据成员与静态成员函数都不专属于某一个对象，是属于整个类。即使没有实例化类的对象，static数据成员和成员函数仍然可以使用
- 【this指针】在static成员中不能使用this指针

## 动态内存分配

通俗来讲：一个对象的生存周期从遇到的第一个大括号开始算
例如：

```cpp
int main()
{
 {//生命周期开始
  Test a;
 }//生命周期结束
 cout << "end of main" << endl;
}
//再例如：
int main()
 {//生命周期开始
  Test a;
 }//生命周期结束
```
## new&delete

和 sizeof 类似，new 和 delete 也不是函数，它们都是 C++ 定义的关键字，通过特定的语法可以组成表达式

Test 为类名

```cpp
Test *pVal = new Test(); 
delete pVal;
pVal = NULL;//一个指针被delete 后，指针赋值为NULL，这是一般的编程规范。
 
Test *pArray = new Test[2];//调用两次构造函数
delete[] pArray;//调用两次析构函数
delete pArray  //这样是不行的！！！！！！
```

## malloc&free

```
 int *p = (int *)malloc(sizeof(int));
 free(p);
 p = NULL; 
 
int *p;
p = (int*)malloc(sizeof(int) * 128);
```

//分配128个（可根据实际需要替换该数值）整型存储单元，
//并将这128个连续的整型存储单元的首地址存储到指针变量p中
double *pd = (double*)malloc(sizeof(double) * 12);
//分配12个double型存储单元，
//并将首地址存储到指针变量pd中\
                                                                                                                  
第一、malloc 函数返回的是 void * 类型。
对于C++，如果你写成：p = malloc (sizeof(int)); 则程序无法通过编译，报错：“不能将 void* 赋值给 int * 类型变量”。所以必须通过 (int *) 来将强制转换。而对于C，没有这个要求，但为了使C程序更方便的移植到C++中来，建议养成强制转换的习惯。 
第二、函数的实参为 sizeof(int) ，用于指明一个整型数据需要的大小。

 


# 2016.5.24
## 构造函数初始化：

构造函数初始化列表以一个冒号开始，接着是以逗号分隔的数据成员列表，每个数据成员后面跟一个放在括号中的初始化式。例如：
构造函数的初始化可用列表方式：Student::Student(int id)：m_id(id),m_score(0)   {  }
                                        等价于： Student::Student(int id)
                                                       {   m_id=id;  m_score=0;     }

## 字符串对象的初始化方法

1.string s1;          默认构造函数，s1为空字符串
2.string s2(s1);    将s2初始化为s1的一个副本
3.string s3("value");    将s3初始化为字符串的副本

4.string s4(n,'c')       将字符串初始化为c的N个副本。

## 字符串对象的操作

s.empty()  如果s为空串,则返回true,否则返回false
s.size()   返回s中字符的个数
s[n]       返回s中位置为n的字符,位置从0开始
s1+s2      把s1和s2连接成一个新字符串,返回新生成的字符串
s1=s2      把s1的内容替换为s2的副本
v1==v2     比较v1与v2的内容,相等则返回true,否则返回false
!=,<,<=,>,>=    保持这些操作符惯有的含义
@string::size_type与std::size_t
string的size_type 是无符号整形（不仅是unsigned int，而是包括unsigned long、unsigned long long等整个整形类别）


```cpp
#ifdef _WIN64
    typedef unsigned __int64 size_t;
    typedef __int64          ptrdiff_t;
    typedef __int64          intptr_t;
#else
    typedef unsigned int     size_t;
    typedef int              ptrdiff_t;
    typedef int              intptr_t;
#endif

#  -------------------------------VS2015中关于size_t的定义                                        
```

区别可参考blog_URL:http://blog.csdn.net/wallwind/article/details/6583714
我们为什么不适用int变量来保存string的size呢？

使用int变量的问题是：有些机器上的int变量的表示范围太小，甚至无法存储实际并不长的string对象。如在有16位int型的机器上，int类型变量最大只能表示32767个字符的string对象。而能容纳一个文件内容的string对象轻易就能超过这个数字，因此，为了避免溢出，保存一个string对象的size的最安全的方法就是使用标准库类型string：：size_type().

## sizeof

sizeof不是函数,是操作符!!!!!!


# 2016.5.22
## 访问对象的成员函数的几种方式

```cpp
//建立指针函数，目的是通过指针调用成员函数
void pointer(Car *pcar)
{
 pcar->run();
 pcar->stop();
}
```

调用：pointer(&benz)

```cpp
//用对象的引用来调用成员函数
void quoet(Car &qcar)
{
 qcar.run();
 qcar.stop();
 qcar.setProperty(int price, int carNum);
}
```

调用：quoet(benz)

## this指针

```cpp
void Car::setProperty(int price, int carNum)
{
 this->m_price = price;
 this->m_carNum = carNum;
}
//用this指针查看对象的地址
void Car::print()
{
 std::cout << "this=" <<this<<std::endl;
}
```

 

# 2016.5.21
## 泛型编程

 泛型编程目的是为实现C++的STL（标准模板库），其语言支持机制就是模板。什么是模板？就是把一个原本特定于某个类型的算法或类当中的类型信息抽掉，抽出来做成模板参数T。
泛型消除了绝大多数的类型转换。如果没有泛型，当你使用集合框架时，你不得不进行类型转换。
关于泛型的理解可以总结下面的一句话，它是把数据类型作为一种参数传递进来

## 函数模板

模板分为函数板和类模板
模板方法为各种逻辑相同但数据类型不同的程序提供代码共享机制。

## 类

类的成员函数可以重载; 
不同的类的成员函数即使有相同的函数名也不算重载。

# 2016.5.20

## 函数重载

 函数重载是具有相同或相似功能的函数使用同一函数名，但这些同名函数的参数类型，参数数量不尽相同。

## 在C++中引用C 文件中定义的函数的用法：

简单总结为extern "C"(可见VS2015 WORKSPACE)

# 2016.5.19
## VS2015快捷键

   注释：先CTRL+K，然后CTRL+C
   取消注释： 先CTRL+K，然后CTRL+U

## 带默认参数的函数

通常是将默认值的设置放在声明中而不是定义中
默认参数的顺序规定：
　　如果一个函数中有多个默认参数，则形参分布中，默认参数应从右至左逐渐定义。当调用函数时，只能向左匹配参数，例如：void fuc(int i=1, int j, int k = 15);是不合法的。


# 2016.5.17

## 宏定义：

```
#define MAX( x, y )   ( ((x) > (y)) ? (x) : (y) )
#define MIN( x, y ) ( ((x) < (y)) ? (x) : (y) )
```

 最外面要加一个括号。
预处理（预编译）工作也叫做宏展开：将宏名替换为字符串。
掌握"宏"概念的关键是“换。
预处理是在编译之前的处理，而编译工作的任务之一就是语法检查，预处理不做语法检查。

## 内联函数：

对功能比较简单，代码较少及使用频率较高的函数，C++引入了内联函数的概念。
inline  返回值类型 函数名（形参）{  }
编译时，类似宏替换，使用函数体替换调用处的函数名。一般在代码中用inline修饰，但是能否形成内联函数，需要看编译器对该函数定义的具体处理。   主要的作用是解决程序的运行效率
1.在内联函数内不允许用循环语句和开关语句。　如果内联函数有这些语句，则编译将该函数视同普通函数那样产生函数调用代码,递归函数(自己调用自己的函数)是不能被用来做内联函数的。内联函数只适合于只有1～5行的小函数。对一个含有许多语句的大函数，函数调用和返回的开销相对来说微不足道，所以也没有必要用内联函数实现。　
2.内联函数的定义必须出现在内联函数第一次被调用之前。
@double float 的区别：
double 和 float 的区别是double精度高，有效数字16位，float精度7位。但double消耗内存是float的两倍，double的运算速度比float慢得多,在不确定的情况下还是尽量用double以保持正确性.



# 2016.5.15
 **引用**

# 2016.5.14
**命名空间**