# TRIVAL

------

### memset( ):

# STL

------
## List(双向循环链表)

- assign() 给list赋值 
- back() 返回最后一个元素 
- begin() 返回指向第一个元素的迭代器 
- clear() 删除所有元素 
- empty() 如果list是空的则返回true 
- end() 返回末尾的迭代器 
- erase() 删除一个元素 
- front() 返回第一个元素 
- get_allocator() 返回list的配置器 
- insert() 插入一个元素到list中 
- max_size() 返回list能容纳的最大元素数量 
- merge() 合并两个list 
- pop_back() 删除最后一个元素 
- pop_front() 删除第一个元素 
- push_back() 在list的末尾添加一个元素 
- push_front() 在list的头部添加一个元素 
- rbegin() 返回指向第一个元素的逆向迭代器 
- remove() 从list删除元素 
- remove_if() 按指定条件删除元素 
- rend() 指向list末尾的逆向迭代器 
- resize() 改变list的大小 
- reverse() 把list的元素倒转 
- size() 返回list中的元素个数 
- sort() 给list排序 
- splice() 合并两个list 
- swap() 交换两个list 
- unique() 删除list中重复的元素

## stack

- top()：返回一个栈顶元素的引用，类型为 T&。如果栈为空，返回值未定义。
- push(const T& obj)：可以将对象副本压入栈顶。这是通过调用底层容器的 push_back() 函数完成的。
- push(T&& obj)：以移动对象的方式将对象压入栈顶。这是通过调用底层容器的有右值引用参数的 push_back() 函数完成的。
- pop()：弹出栈顶元素。
- size()：返回栈中元素的个数。
- empty()：在栈中没有元素的情况下返回 true。
- emplace()：用传入的参数调用构造函数，在栈顶生成对象。
- swap(stack<T> & other_stack)：将当前栈中的元素和参数中的元素交换。参数所包含元素的类型必须和当前栈的相同。对于 stack 对象有一个特例化的全局函数 swap() 可以使用。

## sort

- sort一个二维数组，会按第0维的每一个向量的第一个数字排序
- sort可以对string类型的数据进行排列

## string

https://blog.csdn.net/tengfei461807914/article/details/52203202

```cpp
// 比较,直接用重载的运算符 == 

// 子字符串,其中s为string类型，i为s的起始索引，n为从起始索引到终止索引的长度
s.substr(i, n)
    
//查找成功时返回所在字符串的起始位置，失败时返回-1 
s.find("chen", 0) //从0位置查找字符串"chen"
s.find("chen", 0, 6)//从0至6位置查找字符串"chen"

```

## queue

```
#include<queue>

std::deque<double> values {1.5, 2.5, 3.5, 4.5}
```

back()返回最后一个元素

empty()如果队列空则返回真

front()返回第一个元素

pop()删除第一个元素

push()在末尾加入一个元素

size()返回队列中元素的个数

## bitset

```cpp
#include <bitset>
using std::bitset;

//初始化
bitset<32> foo; //32位，全为0。
//用string对象初始化bitset对象
string strval("1100");
bitset<32> foo(strval);
//用八进制初始化
std::bitset<16> foo (0xfa2);

foo.size() //返回大小（位数）
foo.count() //返回1的个数
foo.any() //返回是否有1
foo.none() //返回是否没有1
foo.set() //全都变成1
foo.set(p) //将第p + 1位变成1
foo.set(p, x) //将第p + 1位变成x
foo.reset() //全都变成0
foo.reset(p) //将第p + 1位变成0
foo.flip() //全都取反
foo.flip(p) //将第p + 1位取反
foo.to_ulong() //返回它转换为unsigned long的结果，如果超出范围则报错
foo.to_ullong() //返回它转换为unsigned long long的结果，如果超出范围则报错
foo.to_string() //返回它转换为string的结果
```



## vector

```cpp
vector<int>test(9,0);//建立一个vector,size为9，全部初始化为0
test.push_back(1);

//访问：使用下标访问元素 + 迭代器访问
vector<int>::iterator it;
for(it=vec.begin();it!=vec.end();it++)
    cout<<*it<<endl;
    
//插入元素：在第i+1个元素前面插入a;
vec.insert(vec.begin()+i,a);

//删除元素：
vec.erase(vec.begin()+2);             //删除第3个元素
vec.erase(vec.begin()+i,vec.end()+j); //删除区间[i,j-1];区间从0开始

//算法，需要头文件#include<algorithm>
reverse(vec.begin(),vec.end()); //将元素翻转，即逆序排列！
sort(vec.begin(),vec.end());//默认是按升序排列,即从小到大)
sort(vec.begin(), vec.end(), greater<int>())

//通过重写排序比较函数按照降序比较：sort(vec.begin(),vec.end(),Comp)
bool Comp(const int &a,const int &b)
{
    return a>b;
}

//查看元素是否存在
std::count(vec.begin(),vec.end(),3);
```



## map

```cpp
//include <map>  //注意，STL头文件没有扩展名.h
map<int, string> mapStudent;
map<char, string> m = { {'2',"abc" },{'3',"def"},{'4',"ghi"} };

//插入数据：insert
mapStudent.insert(pair<int, string>(1, "student_one")); 
mapStudent.insert({1,"student_two"});
//插入数据：数组
mapStudent[1] = "student_one";
mapStudent[1].push_back("student_one")
mapStudent[1].push_back("student_two")

//map的大小
s = mapStudent.size()
    
//查找,注意，map中不存在相同元素，所以返回值只能是1或0。
i = mapStudent.count(1)
    
//find函数，返回的是被查找元素的位置，没有则返回map.end()，注意：map.end()不指向最后一个元素，而指向最后一个元素再+1，当find不到输入的元素时候，返回迭代器就指向这个end() 。
if(auto mapStudent.find(2)== test.end()) {
    cout<<" not found"<<endl;
}

//通过map对象的方法获取的iterator数据类型是一个std::pair对象，包括两个数据 iterator->first 和 iterator->second 分别代表关键字和存储的数据

```

## unorder_map

unordered_map和map类似，都是存储的key-value的值，可以通过key快速索引到value。不同的是unordered_map不会根据key的大小进行排序，存储时是根据key的hash值判断元素是否相同，即unordered_map内部元素是无序的，而map中的元素是按照二叉搜索树存储，进行中序遍历会得到有序遍历。



1 、基本操作

(1)头文件#include\<vector\>

(2)创建vector对象，vector\<int\> vec;

(3)尾部插入数字：vec.push_back(a);

(5)使用迭代器访问元素.

vector\<int\>::iterator it;

for(it=vec.begin();it!=vec.end();it++)

    cout<<*it<<endl;

(6)插入元素：    vec.insert(vec.begin()+i,a);在第i+1个元素前面插入a;

(7)删除元素：    vec.erase(vec.begin()+2);删除第3个元素

vec.erase(vec.begin()+i,vec.end()+j);删除区间[i,j-1];区间从0开始

(8)向量大小:vec.size();

(9)清空:vec.clear();



3、算法
(1) 使用reverse将元素翻转：需要头文件#include\<algorithm\>

reverse(vec.begin(),vec.end());将元素翻转，即逆序排列！

(在vector中，如果一个函数中需要两个迭代器，一般后一个都不包含)

(2)使用sort排序：需要头文件#include\<algorithm\>，

sort(vec.begin(),vec.end());(默认是按升序排列,即从小到大).

sort(vec.begin(), vec.end(), greater\<int\>())   //从大到小排序 

可以通过重写排序比较函数按照降序比较，如下：

定义排序比较函数：

bool Comp(const int &a,const int &b)
{
    return a>b;
}
调用时:sort(vec.begin(),vec.end(),Comp)，这样就降序排序。 

## set

set能根据元素的值自动进行排序。不允许两个元素有相同的键值。在C++中遍历set是从小到大进行的。

```cpp
//构造一个集合
set<T> s;

//插入元素
set<string> country;  
country.insert("China"); // {"China"}
country.insert("America"); // {"China", "America"}

//3、删除元素
country.erase("America"); // {"China"}

//查找元素,想知道某个元素是否在集合中出现，你可以直接用 count( ) 方法--返回某个值元素的个数，如果集合中存在我们要查找的元素，返回 1 ，否则返回 0 。另一种方法是：find()--返回一个指向被查找到元素的迭代器
set<int> set;
for (int i=0; i<list.size(); i++){
	set.insert(list[i]);
}
for (auto i = set.begin(); i != set.end(); i++) {
    cout << *i << endl;
}
//输出：5 14 22 34 39

cout << set.count(5) << endl;
if(set.find(5) != set.end()) 
    cout << *set.find(5) << endl; 
//输出： 1    5

//5、遍历集合
 for (set<string>::iterator it = country.begin(); it != country.end(); ++it) 
        cout << (*it) << endl;
```

## unorder_set

set存放的元素是唯一的，也就是不会有重复的元素，并且是不按特定顺序存放

```cpp
//1、定义一个 string 类型的 unordered_set 为 hash：
unordered_set<string> hash;

//2、在 hash 中插入 s：
hash.insert(s);

//3、在 hash 中删除 s：
hash.erase(s);

//4、hash 中元素个数：
n = hash.size();

//5、取第一个元素、最后一个元素（指针类型），顺序是按照哈希函数之后的顺序：
first = hash.begin();            // 第一个元素的地址
last = hash.end();               // 最后一个元素的地址
std::cout << *first << endl;     // 输出第一个元素
```

