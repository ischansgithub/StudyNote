TensorFlow学习笔记
===



## tf.Session()和tf.Session().as_default()的区别：

- tf.Session()创建一个会话，当上下文管理器退出时会话关闭和资源释放自动完成。

- tf.Session().as_default()创建一个默认会话，当上下文管理器退出时会话没有关闭，还可以通过调用会话进行run()和eval()操作

## GLOBAL_VARIABLES与LOCAL_VARIABLES区别：

- local指定的变量在默认情况下不会保存。即不会保存到checkpoint中

- 定义一个局部变量：需要显示指定所在的变量集合collection

```python
e = tf.Variable(6, name='var_e', collections=[tf.GraphKeys.LOCAL_VARIABLES])
```

## Tensorflow中的命令行参数

```python
flags = tf.app.flags
FLAGS = flags.FLAGS
flags.DEFINE_boolean("enable_colored_log", False, "Enable colored log")
                    "The glob pattern of train TFRecords files")
flags.DEFINE_string("validate_tfrecords_file",
                    "./data/a8a/a8a_test.libsvm.tfrecords",
                    "The glob pattern of validate TFRecords files")
flags.DEFINE_integer("label_size", 2, "Number of label size")
flags.DEFINE_float("learning_rate", 0.01, "The learning rate")

size = FLAGS.feature_size
```



## Anaconda版本

Anaconda3-4.2.0-Windows-x86_64.exe对应Python3.5
Anaconda3-5.2.0-Windows-x86_64.exe对应Python3.6

## 张量(tensor)是什么？

我们可以将标量视为0阶张量；矢量视为1阶张量；矩阵视为2阶张量；一张RGB图片视为三阶张量；n张RGB图片视为四阶张量（n张RGB图片的四个维度分别为：图片编号，高度，宽度，色彩数据。）。
那么其实我们可以理解**张量就是多维数组**。**但张量的实现并不是直接采用数组的形式，它只是对TensorFlow中运算结果的引用。在张量中并没有保存数字，它保存的是如何得到这些数字的计算过程**

```
import tensorflow as tf
a=tf.constant([1.0,2.0],name='a')
b=tf.constant([2.0,3.0],name='b')
result=tf.add(a,b,name='add')
print(result)
```
输出结果为：Tensor(“add:0”, shape=(2,), dtype=float32)。 
所以TensorFlow计算的结果不是一个具体的数字。一个张量主要保存三个属性：名字（name）、维度（shape）和类型（type）。 

## 会话（Session）是什么？

Session是tensorflow里面的重要机制，tensorflow构建的计算图必须通过Session会话才能执行，如果只是在计算图中定义了图的节点但没有使用Session会话的话，就不能运行该节点。比如在tensorflow中定义了两个矩阵a和b，和一个计算a和b乘积的c节点，如果想要得到a和b的乘积（也就是c节点的运算结果)的话，必须要建立Session会话，并调用Session中的run方法运行c节点才行。

```
import tensorflow as tf   
a=tf.constant([[1,2]])#定义了一个1×2的矩阵
b=tf.constant([[2],
               [4]])  #定义了一个2×1的矩阵

c=tf.matmul(a,b)      #matrix_multiply。两个矩阵相乘

sess=tf.Session()
result=sess.run(c)
print(result)
sess.close()          #关闭session，可有可无，但为了规范起见，还是写上
```

## 自编码器(AutoEncoder)是什么？

深度学习中，输入的信息成千上万，要神经网络从庞大的数据中学习是件不容易的事。因此，我们需要提取出最具代表性的信息。自编码器就是提取“最具代表性的信息”的重要工具。

打个比方，自编码器将一张图片打码压缩，再从打码压缩过的图片恢复成原图。其中，就是将打码压缩过的图片放到神经网络进行学习。



ImportError: numpy.core.multiarray failed to import 