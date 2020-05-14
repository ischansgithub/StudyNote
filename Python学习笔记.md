Python学习笔记
===
## 杂乱知识点

 -  一个数组或元组TEMP[1,2,3,4,5],TEMP[1:] = [2,3,4,5,6],而TEMP[:4] = [1,2,3,4]
 -  shoplist = ['apple', 'mango', 'carrot', 'banana'],
 要注意的是 my_shoplist = shoplist 与 my_shoplist = shoplist[:]的区别。
 当my_shoplist = shoplist时，修改my_shoplist也会修改shoplist；
 当 my_shoplist = shoplist[:]时，my_shoplist只是shoplist的一个拷贝，修改my_shoplist并不会影响shoplist
 - 格式化输出是这样的：print('my name is %s, my age is %s' %(self.name, self.age))
   而不是：print('my name is %s, my age is %s'，%self.name， %self.age)
 - python2中的file()对应python3中的open()
 - ''  与 None是不等价的

## functools.lru_cache:实现Python的记忆化
[参考链接]: https://www.cnblogs.com/cuiyubo/p/8375859.html
leetcode关于lru_cache的实战：https://leetcode-cn.com/problems/target-sum/solution/dong-tai-gui-hua-by-powcai-22/
```python
# 多次调用递归会很慢
def fibonacci(n):
    if n==1:
        return 1
    elif n==2:
        return 1
    elif n>2:
        return fibonacci(n-1)+fibonacci(n-2)

for i in range(1,101):
    print(i,":",fibonacci(i)) 


# 解决办法1：记忆法
fibonacci_dic={}

def fibonacci(n):
    if n in fibonacci_dic:
        return fibonacci_dic[n]
    if n==1:
        value= 1
    elif n==2:
        value= 1
    elif n>2:
        value=fibonacci(n-1)+fibonacci(n-2)
    fibonacci_dic[n]=value
    return value

for i in range(1,101):
    print(i,":",fibonacci(i))

# 解决办法2:装饰符
from functools import lru_cache
@lru_cache(maxsize=1000)
def fibonacci(n):
    if n==1:
        return 1
    elif n==2:
        return 1
    elif n>2:
        return fibonacci(n-1)+fibonacci(n-2)
```
## 关键字：nonlocal
在闭包的情况下，内部函数如果要修改外部变量的值，需要将变量声明为 nonlocal

## set用来求交集
```python
valid = set(['yellow', 'red',	'blue',	'green', 'black']) 
input_set=set(['red', 'brown']) 
print(input_set.intersection(valid)) 
#输出: set(['red'])
```
## 内置函数reduce用来计算一个List的和、积
```Python
from functools import reduce 
product	= reduce((lambdax, y: x	* y), \[1, 2, 3,4\]) 
# Output:24
```
## collections.defaultdict(  factory_function )

- defaultdict的存在是为了解决“访问dict中不存在的Key的时候，不会报错”。这个factory_function可以是list、set、str等等，作用是当key不存在时，返回的是工厂函数的默认值，比如list对应[ ]，str对应的是空字符串，set对应set( )，int对应0。
- Python不支持dict的key为list或dict类型，如 a = [1 ,2,3]，dict[a]是不合法的。

## append/extend/insert效率问题

python list 中append的效率是最高的。

## python中的命令行参数

```
import argparse
import sys

parser = argparse.ArgumentParser()
parser.add_argument('--fake_data', nargs='?', const=True, type=bool,
                      default=False,
                      help='If true, uses fake data for unit testing.')
parser.add_argument('--max_steps', type=int, default=1000,
                      help='Number of steps to run trainer.')
parser.add_argument('--learning_rate', type=float, default=0.001,
                      help='Initial learning rate')
parser.add_argument('--data_dir', type=str, default='/tmp/tensorflow/mnist/input_data',
                      help='Directory for storing input data')

FLAGS, unparsed = parser.parse_known_args()
tf.app.run(main=main, argv=[sys.argv[0]] + unparsed)


dir = FLAGS.data_dir
```



## 列表推导式

```shell
>>> L = []
>>> for x in range(1, 11):
...    L.append(x * x)
...
>>> L
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

等价于：

```
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```



进一步地：

```shell
>>> combs = []
>>> for x in [1,2,3]:
	for y in [3,1,4]:
		if x != y:
			combs.append((x,y))
 
>>> combs
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
```

```shell
>>> [(x,y)for x in [1,2,3] for y in [3,1,4] if x != y]
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
```



## range与xrange的区别

range与 xrange的用法一样: range([start,] stop[, step])

不同的是range生成的是一个List对象，而xrange返回的是一个生成器

要生成很大的数字序列的时候，用xrange会比range性能优很多

所以xrange做循环的性能比range好，尤其是返回很大的时候。尽量用xrange吧，除非你是要返回一个列表。

## List, Tuple, Dict, Set的区别

- List : 中括号 [ ] 来表示，有序，查找速度慢，占用内存小
- Tuple : 不可变的list ,用 ( ) 来表示 
- Dict : 花括号 { } 表示 ，无序，查找速度快，占用内存大
- Set : 类似被抽出key的Dict, 内容不能重复。s = set ( ['A',  'B',  'C'] )

## yiled

斐波那契（Fibonacci）數列:除第一个和第二个数外，任意一个数都可由前两个数相加得到.

```python
# 直接在 fab 函数中用 print 打印数字会导致该函数可复用性较差，因为 fab 函数返回 None，其他函数无法获得该函数生成的数列。
def fab(max): 
    n, a, b = 0, 0, 1 
    while n < max: 
        print b 
        a, b = b, a + b 
        n = n + 1
fab(5)
```

```python
# 占用的内存会随着参数 max 的增大而增大，如果要控制内存占用，最好不要用 List
def fab(max): 
    n, a, b = 0, 0, 1 
    L = [] 
    while n < max: 
        L.append(b) 
        a, b = b, a + b 
        n = n + 1 
    return L
 
for n in fab(5): 
    print n
```

```python
# 通过 next() 不断返回数列的下一个数，内存占用始终为常数
# 但代码远远没有第一版的 fab 函数来得简洁
class Fab(object): 
 
    def __init__(self, max): 
        self.max = max 
        self.n, self.a, self.b = 0, 0, 1 
 
    def __iter__(self): 
        return self 
 
    def __next__(self): 
        if self.n < self.max: 
            r = self.b 
            self.a, self.b = self.b, self.a + self.b 
            self.n = self.n + 1 
            return r 
        raise StopIteration()
 
for n in Fab(5): 
    print n
```

```python
def fab(max): 
    n, a, b = 0, 0, 1 
    while n < max: 
        yield b      # 使用 yield
        # print b 
        a, b = b, a + b 
        n = n + 1
 
for n in fab(5): 
    print n
```

总结：yield 的作用就是把一个函数变成一个 generator，带有 yield 的函数不再是一个普通函数，Python 解释器会将其视为一个 generator，调用 fab(5) 不会执行 fab 函数，而是返回一个 iterable 对象！在 for 循环执行时，每次循环都会执行 fab 函数内部的代码，执行到 yield b 时，fab 函数就返回一个迭代值，下次迭代时，代码从 yield b 的下一条语句继续执行，而函数的本地变量看起来和上次中断执行前是完全一样的，于是函数继续执行，直到再次遇到 yield。

## 使用assert替代print

调试须要用到简单粗暴的print，但太多的print会使代码存在太多的垃圾信息。因此可以用assert替代print。

```python
def f(s):
    n = int(s)
    assert n != 0, 'n is zero!' # 断言 n!=0 才是正确的 ，当 n = 0时候会出现 AssertionError: n is zero! 的错误 
    return 10 / n

def main():
    foo('0')
```

与此同时，可以通过 -O 来关闭assert。注意不是数字零，而是大写字母。

```python
# 关闭assert ,assert语句可当成 pass看待
$ python -O err.py 
```

## @property

属性的特点是调用简洁，但是用户可以随意调用并修改；方法的特点是可以检查参数的输入正确与否，但调用不够简洁。@property的作用是把一个方法变成属性调用，将属性与方法的优点结合起来，既能检查参数，调用又简洁。

```python
class Student(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```

```python
>>> s = Student()
>>> s.score = 60 # OK，实际转化为s.set_score(60)
>>> s.score # OK，实际转化为s.get_score()
60
>>> s.score = 9999
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```

## \_\_slot\_\_

python支持动态绑定方法和属性，\_\_slot\_\_的作用在于限制实例的属性。

```python
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```



## 可变参数

如何实现计算a^2 + b^2 + c^2 + d^2 + ...呢？这时候用到可变参数会比较方便。

为什么用可变参数会比较方便呢？

假如不用可变参数：

```python
def calc(numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```

```shell
>>> nums = [1, 2, 3]
>>> calc(nums[0], nums[1], nums[2])
```

也可以

```shell
>>> calc([1, 2, 3])
>>> calc((1, 3, 5, 7))
```

但是都不方便。最方便的是定义一个可变参数

```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```

调用可以非常直接：

```shell
>>> nums = [1, 2, 3]
>>> calc(*nums)
```





## for循环语句

语法格式如下：

```python
for iterater in sequence:
	statements...

//example
for letter in "Python":
	print("当前字母是",letter)

all_fruits = ['banana', 'apple', 'mango']
for fruits in all_fruits:
	print('当前水果'，fruits)
```
## range()函数

range()可以创建一个整数列表，一般用在 for 循环中。
语法：

```python
range(start, stop[, step])
```
参数说明：

 - start: 计数从 start 开始。默认是从 0 开始。例如range（5）等价于range（0， 5）;
 - stop: 计数到 stop 结束，但不包括 stop。例如：range（0， 5） 是[0, 1, 2, 3, 4]没有5
 - step：步长，默认为1。例如：range（0， 5） 等价于 range(0, 5, 1)

实例:
```python
>>>range(10)        # 从 0 开始到 10
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> range(1, 11)     # 从 1 开始到 11
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> range(0, 30, 5)  # 步长为 5
[0, 5, 10, 15, 20, 25]
>>> range(0, 10, 3)  # 步长为 3
[0, 3, 6, 9]
>>> range(0, -10, -1) # 负数
[0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
>>> range(0)
[]
>>> range(1, 0)
[]
```
```python
>>>x = 'runoob'
>>> for i in range(len(x)) :
...     print(x[i])
... 
r
u
n
o
o
b
>>>
```

## continue语句

Python continue 语句跳出本次循环，而break跳出整个循环。

continue 语句用来告诉Python跳过当前循环的剩余语句，然后继续进行下一轮循环。

continue语句用在while和for循环中。

```python
n = 0
while n < 10:
    n = n + 1
    if n % 2 == 0:      # 如果n是偶数，执行continue语句
        continue        # continue语句会直接继续下一轮循环，后续的print()语句不会执行
    print(n)
```
##  列表List

 - 列表是什么？
   列表是一个仓库，不管成员是什么类型，都可以放进去。List = 
   ['123', 123, 3.14, '哈哈']
   
 - 对列表添加元素
   append()：append只能添加一个元素,并且append属于List对象的一个**方法**。List.append("陈炜炜")。
   extend()：extend可以添加多个元素，也是属于List对象的一个**方法**。List.extend(['中国','美国'])。

 - 从列表删除元素
   remove()：对象的**方法**。List.remove('美国')
   del：**非方法**。del List[2]
   pop()：取出序列中栈最顶层的元素。List.pop()  

 - 列表分片slice
   List2 = List[1:5]。创建一个List的儿子，其成员是List中index为1，2 ，3，4的四个元素。

## 元组

Python 的元组与列表类似，不同之处在于元组的元素不能修改。

元组使用小括号，列表使用方括号.

元组创建很简单，只需要在括号中添加元素，并使用逗号隔开即可.

函数可以同时返回多个值，但其实就是一个tuple

## global用法

用以声明全局变量。可省略。有时为增强可读性，强调全局的属性，特意写上。global a = 3是非法的。

## 默认参数值

def func(a, b=5)是有效的，但是def func(a=5, b)是 无效 的,会提醒non-default argument follows default argument

## DocStrings

功能：用来抓取函数的__doc__属性。我的理解是，抓取函数的所有注释。

## python中的继承

Python不会自动调用基本类的constructor，你得亲自专门调用它。

```python
class schoolMem:
    def __init__(self, name, age):
        self.name = name
        self.age  = age
        print('schoolMem \'s init is running')
   
class teacher(schoolMem):
    def __init__(self, name, age, salary):
        #Python不会自动调用基本类schoolMem的constructor:__init__，你得亲自专门调用它。
        schoolMem.__init__(self, name, age) 
        self.salary = salary
```
