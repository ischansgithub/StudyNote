

## keras常用API

------

### **Dense**（全连接层）

```python
keras.layers.core.Dense(units, activation=None )
```

units表示神经元数目

### 激活函数（tanh,sigmoid,relu等）

```python
keras.layers.core.Activation(activation)
```

### Dropout

```python
keras.layers.core.Dropout(rate, noise_shape=None, seed=None)
```

### 卷积

```python
keras.layers.convolutional.Conv2D(filters, kernel_size, strides=(1, 1), padding=‘valid‘,  activation=None)
```

### 反卷积

```python
keras.layers.convolutional.Conv2DTranspose(filters, kernel_size, strides=(1, 1), padding=‘valid‘,  activation=None)
```

### Embedding（嵌入层）

```python
keras.layers.Embedding(input_dim, output_dim, input_length=None)
```

功能：将正整数（索引值）转换为固定尺寸的稠密向量。 例如： [[4], [20]] -> [[0.25, 0.1], [0.6, -0.2]]

该层只能用作模型中的第一层。

**输入：**`(batch_size, sequence_length)` 的 2D 张量。

**输出：** `(batch_size, sequence_length, output_dim)`的 3D 张量

```
# 例子
model = Sequential()
model.add(Embedding(1000, 64, input_length=10))
# 模型将输入一个大小为 (batch=1000, input_length=10) 的整数矩阵。
# 输入中最大的整数（即词索引）不应该大于 999 （词汇表大小）
# 现在 model.output_shape == (1000, 10, 64)，其中 None 是 batch 的维度。
```

### keras 两种训练模型方式fit和fit_generator(节省内存)

```python
model.fit(
x=None, #训练数据
y=None, #训练数据label标签
batch_size=None, #每经过多少个sample更新一次权重，defult 32
epochs=1, #训练的轮数epochs
verbose=1, #0为不在标准输出流输出日志信息，1为输出进度条记录，2为每个epoch输出一行记录
callbacks=None,#list，list中的元素为keras.callbacks.Callback对象，在训练过程中会调用list中的回调函数
validation_split=0., #浮点数0-1，将训练集中的一部分比例作为验证集，然后下面的验证集validation_data将不会起到作用
validation_data=None, #验证集
shuffle=True, #布尔值和字符串，如果为布尔值，表示是否在每一次epoch训练前随机打乱输入样本的顺序，如果为"batch"，为处理HDF5数据
class_weight=None, #dict,分类问题的时候，有的类别可能需要额外关注，分错的时候给的惩罚会比较大，所以权重会调高，体现在损失函数上面
sample_weight=None, #array,和输入样本对等长度,对输入的每个特征+个权值，如果是时序的数据，则采用(samples，sequence_length)的矩阵
initial_epoch=0, #如果之前做了训练，则可以从指定的epoch开始训练
steps_per_epoch=None, #将一个epoch分为多少个steps，也就是划分一个batch_size多大，比如steps_per_epoch=10，则就是将训练集分为10份，不能和batch_size共同使用
validation_steps=None, #当steps_per_epoch被启用的时候才有用，验证集的batch_size
)

#返回的是一个History对象，可以通过History.history来查看训练过程，loss值等等
```

```python
#利用Python的生成器，逐个生成数据的batch并进行训练。生成器与模型将并行执行以提高效率。例如，该函数允许我们在CPU上进行实时的数据提升，同时在GPU上进行模型训练
model.fit_generator(
generator, #生成器函数
steps_per_epoch, #整数，当生成器返回steps_per_epoch次数据时计一个epoch结束，执行下一个epoch
epochs=1, 
verbose=1, #0为不在标准输出流输出日志信息，1为输出进度条记录，2为每个epoch输出一行记录
callbacks=None, 
validation_data=None, 
validation_steps=None, 
class_weight=None, 
max_q_size=10, # 从生产函数中出来的数据时可以缓存在queue队列中
workers=1, 
pickle_safe=False, 
initial_epoch=0)
```

