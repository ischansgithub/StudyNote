#《Effective C++》学习笔记

##条款30：透彻了解inlining的里里外外
###(一)一些小细节

 - 过度热衷inline可能会造成代码膨胀。
 -  inline只是对编译器的一个申请，不是强制命令。在一些情况下，编译器可以忽略申请，比如你想要将过于复杂(循环或递归)inlining，或者想要对virtual函数inlining。众所周知，virtual函数意味着在运行期间才确定调用哪个函数，而inline在执行前就要被替换为"函数本体"
 -  不要试图对构造函数/析构函数inlining。

---
##条款29：为“异常安全”而努力是值得的
###(一)什么叫“异常安全”？
当异常被抛出时，带有异常安全性的函数会：

- 不泄漏任何资源
- 不允许数据败坏
    

###(二)对应上述两种情况我们要怎么做？
- 为避免泄漏资源，我们一般用对象来管理资源(结束后自己调用析构函数)
- 为了不让数据败坏，我们通常要遵守以下三个保证

    1.基本承诺
    2.强烈保证
    3.不抛掷保证
###(三)关于三个保证
- 基本承诺的概念是一旦有异常被抛出，那么对象可以继续拥有原有资源，或是拥有某个缺省资源，但客户无法预期是哪一种情况。
- 强烈保证的概念是如有异常被抛出，程序状态不变。即成功便是完全成功，失败，则会回到原始的状态。
- 不抛掷保证是使绝不抛出异常

###(四)强烈保证的做法

 - 用智能指针std::trl::shared_ptr来管理。当须要delete一个旧资源，再new一个新资源时，不必手动delete旧资源。
 -  "copy and swap原则"。当你打算修改对象时，先做一个副本，然后在副本上做修改，等到所有修改都成功后(即没有抛出异常)，将副本与原对象swap。

###(五)一段写得挺好的话P131
四十年前，满载goto的代码被视为一种美好实践，而今我们却致力写出结构化控制流。二十年前，全局数据被视为一种美好实践，而今我们却致力于数据的封装。十年前，撰写“未将异常考虑在内”的函数被视为一种美好实践，而今我们致力于写出“异常安全码”。

时间不断前进。我们与时俱进！

---
##条款28：避免返回handles指向对象内部成分
###(一)为什么要避免返回handles指向对象内部成分？
阅读以下代码
```
class Point
{
public:
    Point(int _x,int _y) {}
    void setX(int _x) {x=_x};
    void setY(int _y) {y=_y};
};

struct RectData
{
    Point ulhc;//左上角
    Point lrhc;//右下角
};

class Rectangle
{
public:
    Point& upperleft() const {return pData->ulhc;}
    Point& lowerRight() const {return pData->lrhc;}
private:
    std::trl::shared_ptr<RectData> pData;
};
```

```
Point& upperleft() const {return pData->ulhc;}
Point& lowerRight() const {return pData->lrhc;}
```
上述程序的本意是不允许函数修改Rectangle,但实际两个函数都返回引用，从而指向对象内部成分

那么
```
Point coord1(0,0);
Point coord2(100,100);
const Rectangle rec(coord1,coord2);

rec.upperleft().setX(50);//如果函数这样写那么就完全能够违背初衷！
```
###(二)如何解决上述问题？

```
const Point& upperleft() const {return pData->ulhc;}
const Point& lowerRight() const {return pData->lrhc;}
```
###(三)但是还是违背了原则，还是返回了对象内部的成分。因为这会造成handling handles
```
class GUIObject {};

const Rectangle boundingBox(const GUIObject& obj)
{
    //by value的方式返回Rectangle类型
}

//那么，以下代码会发生什么 、？
GUIObject* pgo;
const Point* pUpperLeft=&(boundingBox(*)pgo).upperLeft);

//最终pUpperLeft指向一个被销毁的东西，变成了dangling handles
```
###(四)总结
evc切记避免返回handles(reference,指针等)指向对象内部

---
##条款27：尽量少做转型动作
###(一)四种转型动作

 1. const_cast<T>(expression) ===>将对象由const转化为non-const
 2. dynamic_cast<T>(expression)===>“安全向下转移”(ps.个人理解。通俗来讲就是在继承体系中，将父类转化为子类。)
 3. reinterpret_cast<T>(expression)===>少用，不管
 4. static_cast<T>(expression)===>non-const转化为const/intl转化为double/void*指针转化为typed指针

###(二)关于static_cast转型有趣的代码
许多应用框架(application frameworks)都要求derived        class的virtual函数第一个动作调用base class的对应函数。
下面的程序，看起来对，实际是错误的！！
```
class Window{
public:
    virtual void onResize() { ... }
};

class SpecialWindow:public Window{
public:
    virtual void onResize(){
        //企图将*this指针转化为Window类型，并调用Window的onResize函数
        static_cast<Window>(*this).onResize();
        //正确做法
        Window::onResize();
        }
};
```

分析：为什么static_cast<Window>(*this).onResize()的做法错误？
答：这样的做法并不是调用当前对象，而是转型动作所建立的一个“*this对象之base class成分”的暂时副本身上的onResize!所以这样做当前对象并没有真正执行base class的onResize！因为执行base class的onResize函数的是一个副本！！
###(三)dynamic_cast的用法及如何优化
通常想用dynamic_cast是想要在derived class对象上执行derived class的操作函数 ，但是手头却只有一个指向base class的指针。

方法一：用智能指针直接指向derived class

但首先，不要试着如下这样做：
```
class Window{};

class SpecialWindow
{
public:
    void blink();
};

typedef std::vector<std::trl::shared_ptr<Window>> VPW;

VPW winptr;

for(VPW::iterator iter=winptr.begin();iter!=winptr.end();iter++)
{
    //想要将父类指针转型并赋给一个新创建的子类指针
	if(SpecialWindow* psw=dynamic_cast<SpecialWindow*>(iter->get()))
		psw->blink();
}
```
秉承尽量少做转型动作的原则，代码应如下这么写
```
class Window{};

class SpecialWindow
{
public:
    void blink();
};

typedef std::vector<std::trl::shared_ptr<SpecialWindow>> VPSW;

VPSW winptr;

for(VPSW::iterator iter=winptr.begin();iter!=winptr.end();iter++)
{
	(*iter)->blink
}
```

方法二：用智能指针直接指向base class,但函数用virtual
```
class Window{};

class SpecialWindow
{
public:
    virtual void blink();
};

typedef std::vector<std::trl::shared_ptr<Window>> VPW;

VPW winptr;

for(VPW::iterator iter=winptr.begin();iter!=winptr.end();iter++)
{
	(*iter)->blink();
}
```
###(四)原则

  尽量避免转型，特别是注重效率的话避免使用dynamic_cast

---
##条款26：尽可能延后变量定义式出现的时间

###(一)为什么要尽可能延后变量定义式出现的时间？
例如以下的程序
```
std::string encryptPassword(const std::string& password)
{
    using namespace std;
    string encrypted;//定义一个加密变量
    if(password.length<MinimumPasswordLength){//如果传入密码长度小于某值
    throw logic_error("Password is too short");
    }
    
    //接下来一系列动作将加密的密码置入变量encrypted.并返回
    return encrypted;
}
```
假如函数传入的密码参数过短，那么就会抛出异常。那么定义的encrypted就不会返回，相当于encrypted定义了却完全没有用到。

那么，就要白白付出encrypted的构造及析构成本！！

因此应该尽可能延后变量定义式出现的时间。如下：
```
std::string encryptPassword(const std::string& password)
{
    using namespace std;
    //string encrypted;
    if(password.length<MinimumPasswordLength){//如果传入密码长度小于某值
    throw logic_error("Password is too short");
    }
    
    string encrypted;//直到非得使用该变量才定义这一个加密变量
    //接下来一系列动作将加密的密码置入变量encrypted.并返回
    return encrypted;
}
```
###(二)那么如果涉及到循环呢？
```
//方法A:定义变量于循环体外
Widget w;
for(int i=0;i<n;i++)
{
    w=i;
}

//方法B:定义变量于循环体内
for(int i=0;i<n;i++)
{
    Widget w;
    w=i;
}
```
这样的话，哪个更好？

分析：
    A:1个构造函数，1个析构函数，n个赋值操作
    B:n个构造函数，n个析构函数，n个赋值操作

A操作，w的作用域比B大，会对程序的可理解性及易维护性造成冲突。
B操作，n值很大时候，B操作效率更高。

所以，如果对效率高度敏感的话，用A更适合。否则，用B。

---
##条款25：考虑写出一个不抛异常的swap函数
###(一)什么是pimpl手法？

pimpl(pointer to implementation)指“以指针指向一个对象，内含真正的数据”

通俗来讲，就是建立两个类，类A包含有许多数据，类B中建立一个类型为A的指针，通过这个指针指向类A，从而调用类A中的数据(成员变量)

###(二)pimpl手法的好处？

如果须要对一个对象进行复制，势必要复制其中的所有资源。当一个类中的资源(或者可以说是成员变量较多时？)，复制所有资源缺乏效率性。

当用pimpl手法后，只须要对指针进行操作。

```
//赋值运算符
Widget& operator=(const Widget& rhs)
{
    *pImpl=*(rhs.pImpl);
}
```

###(三)怎么实现高效的swap，从而实现交换两个对象的功能？
```
using namespace WidgetStuff{
	
	//数据存储类
	template<typename T>
	class WidgetImpl{
	public:
		//...
	private:
		T a,b,c,d,e,f,g//很多数据
	};
	
	template<typename T>
	class Widget{
	public:
		//member swap 函数 
		void swap(Widget& other)
		{
			*pImpl=*(other.pImpl);
		}
	private:
		WidgetImpl<T>* pImpl;	
	};
	
	//non-member 函数
	template<typename T>
	void swap(Widget<T>&  a,Widget<T>& b)
	{
		a.swap(b);//通过non-member 函数调用member 函数
	}
}
```

###(四)值得注意的地方

std的内容由C++标准委员会决定，标准委员会禁止用户膨胀已经声明好的东西。所以不要试图在std内加入某些对std而言全新的东西(虽然还是可以编译和执行)

---
##条款24：若所有参数皆需类型转换，请为此采用non-member函数

###(一)所有参数皆需类型转换是什么意思？
一般情况下设计class都不鼓励支持隐式类型转换。但有例外，当你设计一个class用来建立**数值类型**的时候，需要支持内置类型转换为自己设计的数值类型，这时候class要支持类型转换。
在这种情况下，class内部的参数都需要类型转换。

###(二)若所有参数皆需类型转换，采用member函数会怎样？

```
class Rational{
public:
    Rational(int n,int d);     //non-explict
    int numerator() const;     //分子
    int denominator() const;   //分母
    const Rational operator*(const Rational& rhs) const;
};
```
那么此时我们需要隐式类型转换的时候。

```
Rational oneEighth(1,8);
Rational result=oneEighth*2   // 2隐式转换为Ratioanl temp(2),OK。
                              // 相当于oneEighth.operator*(temp)。

Rational result=2* oneEighth  // ERROR!!!2没有相应的class
```

###(三)若所有参数皆需类型转换，采用non-member函数会怎样？
    
```
namespace RationalNamespace{
    class Rational{
    public:
        Rational(int n,int d);     //non-explict
        int numerator() const;     //分子
        int denominator() const;   //分母
    };
    
    //non-member
    const Rational operator*(const Rational& lhs,const Rational& rhs)
    {
        return Rational(lhs.numerator()*rhs.numerator(),lhs.denominator()*rhs.denominator());
    }
}
```
那么此时我们需要隐式类型转换的时候。
```
Rational oneEighth(1,8);
Rational result=oneEighth*2   // 2隐式转换为Ratioanl temp(2),OK。
                              // 相当于oneEighth.operator*(temp)。

Rational result=2* oneEighth  // 也OK!
```

----------


##条款23：宁以non-member non-friend函数替换member函数

###(一)那么什么时候non-member non-friend函数替换member函数比较好呢？

答：当需要一个能提供便利的函数，一次性对某个class里的多个方法函数进行操作。

比如某个class：

```
class WebBrowser{
    public:
        void clearCache();      //清理缓存
        void clearHistory();    //清理历史
        void removeCookies();   //清理cookies
    }
```
现在需要一个函数clearEverything一次性进行清理工作，就需要考虑到这个函数要作member函数还是作non-member函数。

###(二)那为什么用non-member non-friend函数替换member函数比较好呢？

 1. 增加封装性
 2. 增加包裹弹性
 3. 机能扩充性

1.增加封装性
    面对对象守则要求数据应该尽可能地被封闭。
    通俗来讲，一个类中越多的成员函数能够访问成员变量，那么说明这个类的数据封闭性越低。
    由此可得出，在现有情况下用non-member函数更能增加封闭性，因为这个non-member函数并不能直接访问类中的成员变量。而相反地，member函数却能直接访问类中的成员变量。

2.增加包裹弹性
    当前情况下，可以将class以及non-member便利函数通过namespace包裹起来，并可以通过多个头文件进行non-member便利函数与class的不同包裹。
比如
```
    //头文件webbrowser_cache.h
    namespace WebBrowserStuff{
    class WebBrowser{};
    clearThingAboutCache{};  //清除与Cache有关的垃圾的相关函数
    }
    
    //另一个头文件webbrowser_history.h
    namespace WebBrowserStuff{
    class WebBrowser{};
    clearThingAboutHistory{};  //清除与history有关的垃圾的相关函数
    }
```

从而可以降低编译依赖性。
而相反地，member的便利函数则不能如namespace一样被分割。

3.机能扩充性
     可以让客户利用namespace扩展函数而不是直接在class里进行扩展。
     
----------

##条款22: 将成员变量声明为private

###为什么要将成员变量声明为private?

1.语法一致性:这样就不用考虑访问成员函数时到底要不要加括号。因为当所有成员变量都是private后，为public的都是函数。

2.能更好更细微地进行访问控制，如通过一函数可以限定一个成员变量的属性，只读/读写/惟写。。。

















