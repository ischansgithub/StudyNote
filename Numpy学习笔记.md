NumPy学习笔记
===



## numpy 无法不指定形状地去创建数组

## range与numpy.arange

range 是python的内置函数，返回List

numpy.arange返回的是ndarray

## array和asarray的区别

当数据源是ndarray时，array仍然会copy出一个副本，占用新的内存，但asarray不会copy副本也不会占用新的内存。

```python
data1=[[1,1,1],[1,1,1],[1,1,1]]
arr2=np.array(data1)
arr3=np.asarray(data1)
data1[1][1]=2
print 'data1:\n',data1
print 'arr2:\n',arr2
print 'arr3:\n',arr3
```

```shell
# 输出
data1:
[[1, 1, 1], [1, 2, 1], [1, 1, 1]]
arr2:
[[1 1 1]
 [1 1 1]
 [1 1 1]]
arr3:
[[1 1 1]
 [1 1 1]
 [1 1 1]]
```



## tpye与dtype的区别

type(a)一个返回的是a 的数据类型 np.array  

 a.dtype 返回的是数组中内容的数据类型

```shell
>>> a =np.array([[1,2],[3,4]])
>>> type(a)
<class 'numpy.ndarray'>
>>> a.dtype
dtype('int32')
```

## axis

axis=0，表示沿着第 0 轴进行操作，即对每一列进行操作；axis=1，表示沿着第1轴进行操作，即对每一行进行操作。

## 为什么需要NumPy?

 - python中的list可作数组使用，但是list中的元素可以是任何对象，因此list保存的是对象的指针，对于数值运算来说**不够高效**
 - python中的array仅支持一维数组。

## NumPy数组属性

NumPy的数组中比较重要ndarray对象属性有：

- ndarray.ndim：数组的维数（即数组轴的个数），等于秩。
- ndarray.shape：数组的维度。对于矩阵，n行m列
- ndarray.size：数组元素的总个数，等于shape属性中元组元素的乘积。
- ndarray.dtype：表示数组中元素类型的对象，可使用标准的Python类型创建或指定dtype。
- ndarray.itemsize：数组中每个元素的字节大小。例如，一个元素类型为float64的数组itemsiz属性值为8(float64占用64个bits，每个字节长度为8，所以64/8，占用8个字节），又如，一个元素类型为complex32的数组item属性为4（32/8）。

## 运用array函数创建数组

```
>>> a = array(1,2,3,4)    # 错误  
>>> a = array([1,2,3,4])  # 正确 
```

## 运用zeros,ones函数创建数组

```
>>> a = zeros((2,3))       
>>> a
 array([[0., 0., 0.],
       [0., 0., 0.]])


>>> a = zeros(2)
>>> a
array([0., 0.])

#ones与zeros的用法相同。
```

## import numpy 和 from numpy import  \* 的区别

import numpy，如果使用numpy的属性都需要在前面加上numpy

from numpy import *，则不需要加入numpy

后者不建议使用，如果下次引用和numpy里的函数一样的情况，就会出现命名冲突。