### torch.nn.Module.eval() 和 torch.nn.Module.train() 的作用

model.train() ：启用 BatchNormalization 和 Dropout

model.eval() ：不启用 BatchNormalization 和 Dropout

### 进行推理时，GPU显存占满，但是GPU利用率不高

进行分类推理时，发现GPU的显存占满，但是占用率最多到23%，不能达到100%。原因在于我代码实现的方法不对：喂入神经网络的数据是一张一张的。而要想GPU利用率高，就要用tf.concat将多张图片进行拼接，再送入神经网络。

### tf.train.batch和tf.train.shuffle_batch

tf.train.batch([example, label], batch_size=batch_size, capacity=capacity)：[example, label]表示样本和样本标签，这个可以是一个样本和一个样本标签，batch_size是返回的一个batch样本集的样本个数。capacity是队列中的容量。这主要是按顺序组合成一个batch.

tf.train.shuffle_batch([example, label], batch_size=batch_size, capacity=capacity, min_after_dequeue)。这里面的参数和上面的一样的意思。不一样的是这个参数min_after_dequeue，一定要保证这参数小于capacity参数的值，否则会出错。这个代表队列中的元素大于它的时候就输出乱的顺序的batch。也就是说这个函数的输出结果是一个乱序的样本排列的batch，不是按照顺序排列的。

上面的函数返回值都是一个batch的样本和样本标签，只是一个是按照顺序，另外一个是随机的

### 召回率和准确率

![](F:\学习笔记与学习资料\学习笔记\assets\recall_accuracy.jpg)





### shell脚本执行错误 $'\r':command not found

在linux上执行脚本时出现$’\r’:command not found,然而仔细检查脚本，对应行位置只是一个空行，并没有问题，那么linux为什么会将一个回车的空行报错？

原因是这样的：脚本是在window下编辑完成后上传到linux上执行的，win下的换行是回车符+换行符，也就是\r\n,而unix下是换行符\n。linux下不识别\r为回车符，所以导致每行的配置都多了个\r，因此是脚本编码的问题。

在linux上执行 dos2unix 脚本名，再次执行脚本，报错消失。

------





函数freeze_graph中，最重要的就是要确定“指定输出的节点名称”，这个节点名称必须是原模型中存在的节点，对于freeze操作，我们需要定义输出结点的名字。因为网络其实是比较复杂的，定义了输出结点的名字，那么freeze的时候就只把输出该结点所需要的子图都固化下来，其他无关的就舍弃掉。因为我们freeze模型的目的是接下来做预测。所以，output_node_names一般是网络模型最后一层输出的节点名称，或者说就是我们预测的目标。

注意节点名称与张量的名称的区别，例如：“input:0”是张量的名称，而"input"表示的是节点的名称。



### 查看pb文件中所有网络节点的名称

- 法一

```shell
bazel build tensorflow/tools/graph_transforms:summarize_graph

bazel-bin/tensorflow/tools/graph_transforms/summarize_graph \
  --in_graph=/tmp/inception_v3_inf_graph.pb
```

其中bazel可以在服务器中通过locate bazel 来看看有没有bael这个可执行文件

- 法二：

```python
import tensorflow as tf
import sys
from tensorflow.python.platform import gfile

from tensorflow.core.protobuf import saved_model_pb2
from tensorflow.python.util import compat
with tf.Session() as persisted_sess:
  print("load graph")
  with gfile.FastGFile("./latest.pb",'rb') as f:
    graph_def = tf.GraphDef()
    graph_def.ParseFromString(f.read())
    persisted_sess.graph.as_default()
    tf.import_graph_def(graph_def, name='')
    writer = tf.summary.FileWriter("./tf_summary", graph=persisted_sess.graph)
    # Print all operation names
    for op in persisted_sess.graph.get_operations():
      print(op)
    # next: do the following in bash:
    # tensorboard --logdir ./tf_summary/
```



### 查看checkpoint中所有网络节点的名称