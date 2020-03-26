## 代码解析

### Discriminator卷积层

```python
def discrim_conv(batch_input, out_channels, stride):
    padded_input = tf.pad(batch_input, [[0, 0], [1, 1], [1, 1], [0, 0]], mode="CONSTANT")
    return tf.layers.conv2d(padded_input, 
                            out_channels, 
                            kernel_size=4, 
                            strides=(stride, stride),                  padding="valid",kernel_initializer=tf.random_normal_initializer(0, 0.02))				                                     
```

tf.pad对batch_input的每一维进行填充，对batch及chanel维不进行填充，对二维的图像矩阵上下左右分别填充1，1，1，1行（列）的0