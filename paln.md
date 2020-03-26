学习目标:C/C++、操作系统、Linux驱动

待安排

- [ ] 《tensorflow实战》VGGNet
- [ ] 《tensorflow实战》AlexNet
- [x] CS231n：方向传播
- [ ] 学习tf-slim 怎么进行迁移训练 

### 2020.02.19

TPDPS/TCOTS/TTFBG/需要提高准确率。因为这几个code需要turn on

### 2020.01.16

- [ ] 整理二叉树前中序遍历（递归+迭代）
- [ ] 分析图片数据
- [ ] 

### 2020.01.12

- [x] 整理单词搜索
- [x] 写检测模型推理生成xml的代码

### 2020.01.1

- [ ] 修改nms函数
- [x] 看完目标检测综述
- [x] 刷题

### 2020.01.14

- [x] 用训练好的检测分类模型测试训练集，挑出第二自信度>0.1 、>0.2、及分类错误的图片分析，想要能过阈值，将一些不好判断的数据剔除训练集。
- [x] leetcode两题

### 2020.01.10

- [x] spp-net了解
- [x] yolo源码-前向传播：阅读理解调试
- [ ] 清洗图片，训练分类
- [x] leetcode两题目

### 2019.12.31

- [x] 768*1024 pre +  ots
- [x] read source code

### 2019.12.27

- [x] 训练检测模型V2
- [x] 重新标注GS0，并进行训练 （v3)
- [x] 刷题

### 2019.12.26

- [x] 实现kmeans 函数并替代coco的anchor
- [x] 模板分割/缺陷分割 代码运行学习
- [x] 刷题
- [x] 查看t2数据

### 2019.12.23

- [ ] 整理图片的问题，准备到厂商现场解决
- [ ] 解决检测模型的训练问题
- [ ] 模拟检测测试集的推理
- [ ] 模拟分类测试集的推理

### 2019.12.20

- [x] 只出现 一次的数
- [x] unorder_set 取出元素
- [x] *us.begin();
- [x] 训练新版本，分类模型

### 2019.12.19

- [x] 整理c++ vector 
- [x] 整理 打家劫舍
- [x] 分类

### 2019.12.13

- [ ] OTS不知道如何清洗：目前只清洗模糊、看着没有缺陷类



```python
# 1213_5cls_v2_clean_mr_ep3k_bs8/result149

thresh: 0.00 recall: 1.00 precision: 0.80
        TTP2S   TDINR   TCFBA   TEOTS   TGGS0   NONO 
TTP2S   |455     0       12      35      9       0              511     0.89
TDINR    0      |171     6       35      1       0              213     0.80
TCFBA    18      7      |158     12      3       0              198     0.80
TEOTS    35      30      17     |129     7       0              218     0.59
TGGS0    26      0       7       15     |193     0              241     0.80
         0.85    0.82    0.79    0.57    0.91 
```



### 2019.12.12





- [x] 训练一版去除小碎框标注集前的目标检测
- [x] 训练一版去除小碎框标注集后的目标检测
- [x] 训练一版去除小碎框标注集后的分类模型
- [x] 直接xml->yolo-lable一步到位

### 2019.12.11

- [ ] 重新标注T7数据
- [ ] 3800_TH991393AE_THAOS120_1072_0801.755_-0867.717_2048X2048___M_COLOR_20190920_055342.jpg   2S -> GS0
- [ ] 3800_TH991393AL_THAOS120_2_-1128.512_1436.567_2048X2048___M_COLOR_20190920_055623.jpg    2S ????
- [ ] 3800_TH991482AN_THAOS120_636_1090.753_-0659.233_2048X2048___S_COLOR_20190921_110005.jpg  2s->GS0
- [ ] 3800_TH991567AJ_THAOS220_118_0648.356_-0254.900_2048X2048___M_COLOR_20190922_041159  2S -> gs0?
- [ ] 3800_TH991567AP_THAOS220_129_0999.171_-0757.489_2048X2048___S_COLOR_20190922_041311.jpg   2S -> GS0
- [ ] 3800_TH991569AT_THAOS220_163_0030.711_-0581.672_2048X2048___M_COLOR_20190922_040611.jpg   2s?   GS0?

### 2019.12.10

- [x] 对比三种不同crop的结果
- [x] OTS数据清洗完毕，继续清洗接下来的数据 

### 2019.12.09

等noobj=1的结果出来 后关注这张图（划痕缺陷，但是noobj=10只能出一个小框）：

3800_TH9A0028AA_THAOS120_512_-0954.505_-1562.219_2048X2048___L_COLOR_20191004_015956.jpg



对检测错误的图片的1/3进行图片增强？

分类：多个bbox

### 2019.2.06

今日计划：

- 检测：
  - [x] 代码加入策略，将IOU=0的验证图片单独保存，重新加入训练
- 分类：
  - [ ] 类均衡
  - [x] 多个标注框，后重新训练
  - [ ] 分析全部类的数据
  - [ ] 清洗OTS
  - [ ] 清洗其他类数据

今日进度：

- 检测：

  - 训练noobj_scale=1模型 。ing

  - 目前来看：noboj_scale=10会比1效果好

  - 训练去除TSFAS的模型，问题：recall反而往下掉？

  - ![](F:\T7\yolo_v3_ep39_lr0-001.PNG)

  - 

    ```
    去除FAS类后
    yolov3_ckpt_40.pth
    Test: 39[>0.10] val_precision: 0.20	val_recall: 0.80	val_mAP: 0.78	val_f1: 0.32
    Test: 39[>0.30] val_precision: 0.46	val_recall: 0.78	val_mAP: 0.77	val_f1: 0.58
    Test: 39[>0.50] val_precision: 0.78	val_recall: 0.78	val_mAP: 0.77	val_f1: 0.78
    Test: 39[>0.70] val_precision: 0.94	val_recall: 0.74	val_mAP: 0.74	val_f1: 0.83
    ```

    

- 分类：

  - ​	5cls分类继续在2000ep的基础上再训练2000ep

  - ```
    #结果
    
    6cls_ep999
    thresh: 0.00 recall: 1.00 precision: 0.74
    thresh: 0.10 recall: 1.00 precision: 0.74
    thresh: 0.20 recall: 1.00 precision: 0.74
    thresh: 0.30 recall: 0.98 precision: 0.75
    thresh: 0.40 recall: 0.94 precision: 0.76
    thresh: 0.50 recall: 0.87 precision: 0.79
    thresh: 0.60 recall: 0.78 precision: 0.83
    thresh: 0.70 recall: 0.69 precision: 0.86
    thresh: 0.80 recall: 0.59 precision: 0.89
    thresh: 0.90 recall: 0.47 precision: 0.93
    thresh: 0.99 recall: 0.13 precision: 0.98
    
    
    5cls_ep2000
    thresh: 0.00 recall: 1.00 precision: 0.77
    thresh: 0.10 recall: 1.00 precision: 0.77
    thresh: 0.20 recall: 1.00 precision: 0.77
    thresh: 0.30 recall: 0.99 precision: 0.77
    thresh: 0.40 recall: 0.99 precision: 0.78
    thresh: 0.50 recall: 0.97 precision: 0.78
    thresh: 0.60 recall: 0.96 precision: 0.79
    thresh: 0.70 recall: 0.92 precision: 0.80
    thresh: 0.80 recall: 0.88 precision: 0.82
    thresh: 0.90 recall: 0.82 precision: 0.84
    thresh: 0.99 recall: 0.61 precision: 0.90
    
    
    5cls_ep2199
    thresh: 0.00 recall: 1.00 precision: 0.80
    thresh: 0.10 recall: 1.00 precision: 0.80
    thresh: 0.20 recall: 1.00 precision: 0.80
    thresh: 0.30 recall: 0.99 precision: 0.80
    thresh: 0.40 recall: 0.99 precision: 0.80
    thresh: 0.50 recall: 0.98 precision: 0.80
    thresh: 0.60 recall: 0.96 precision: 0.81
    thresh: 0.70 recall: 0.94 precision: 0.82
    thresh: 0.80 recall: 0.91 precision: 0.83
    thresh: 0.90 recall: 0.86 precision: 0.84
    thresh: 0.99 recall: 0.71 precision: 0.89
    
    
    thresh: 0.10 recall: 1.00 precision: 0.80
            TTP2S   TEOTS   TCFBA   TGGS0   TDINR   NONO 
    TTP2S   |483     29      21      26      0       0              559     0.86
    TEOTS    33     |163     21      8       41      0              266     0.61
    TCFBA    26      23     |202     4       7       0              262     0.77
    TGGS0    25      13      0      |211     4       0              253     0.83
    TDINR    2       27      11      0      |211     0              251     0.84
             0.85    0.64    0.79    0.85    0.80  
    
    
    ```

明日计划：

### 2019.12.04

今日计划：

- [x] 全局代码整理
- [x] T7目标检测数据生成代码整理
- [x] 分析分类数据
- [x] 训练 6分类模型：TTP2S3S合并

今日进度：

明日计划：



### 2019.12.03

- 今日进度：
  - 训练T7-6分类模型
  - 分析T7图片数据
  - 验证T7-7分类模型；验证T7目标检测模型
- 明日计划：
  - 写脚本生成目标检测可视化结果
  - T7目标检测数据生成代码整理
  - 训练 6分类模型：TTP2S3S合并

### 2019.12.02

- 进度：

  - 训练T7目标检测模型
  - 训练T7分类模型

- 问题：

  - 解决T7目标检测训loss为nan，原因有两个，第一是程序有点问题，一个标志位向量有时候会全为0，导致?索引为空；第二个是标注数据问题，有的图片有标注文本xml，但是没有标注框的信息，从xml转成yolo-label的时候会（x,y,w,h）会变成（-1，-1，-1，-1）,计算Loss的时候log(负数)。

- 计划：

  - [ ] T7目标检测数据生成代码整理
  - [x] 分析图片数据
  - [ ] 写脚本生成可视化结果
  - [x] 跑通推理程序、研究推理程序

- 收获：

  - " invalid literal for int() with base 10" 错误在于将str类型直接强制转换成int。正确的做法是str -> float -> int

  - ```python
    x1 = min(max(0, x1), w - sz)
    y1 = min(max(0, y1), h - sz)
    ```

    巧妙解决边界问题！！！！

### 2019.11.30

- [x] 修改train.dat/test.dat/的生成。

- [ ] 两个代码跑起来


- [ ] cls:dataset.py from imgaug import augmenters as iaa


### 2019.11.29

- [x] 确定阈值

### 2019.11.26

- [x] 训练，验证，测试mura二分类模型初版
- [x] 训练mura55分后的二分类模型
- [ ] 整理tf-slim图片数据到tfrecord的程序
- [x] leetcode编程题目
- [ ] 报告PPT撰写

### 2019.11.25

- [x] 尝试mask训练新策略：训练mask原图片中左下角的数据，到step:20000,测试结果除了FET类（孔不均匀）相对正常，其他都不正常，几乎偏向EMDFKL（被黑色挡住）
- [x] 今日收获：vector中a.back()  等于 python中的pop

### 2019.11.22

- [x] 今日收获：c++ algorithm中的sort(itor,itor+n)
- [x] 今日收获: c++  for(auto i:vec) { ...... }

### 2019.11.21

- [x] GLASS\MASK联合检测阈值的测试评估

### 2019.11.20

- [x] 解决自己生成tfrecord的时候，想要生成train，但会生成val，与些同时还会删掉原图片。

### 2019.11.19

- [x] gen_defect_free_data.py    mask   一张图多采几张负样本，再缩小一点图片占比,64 - 100?
- [x] 分析mask glass检测分数低的样本特征，从随机生成的负样本中筛选符合检测的负样本的分布


### 2019.11.15

- [ ] 整理写过的脚本，尽可能使其具有通用性
- [ ] draw and save from json 整理成一个函数
- [x] 继续研究leetcode 及 滑动窗口法
- [x] 验证、测试新训练的GLASS/MASK模型

### 2019.11.14

- [x] MASk/GLASS无缺陷进一步数据清洗

### 2019.11.11

- [x] 总结遇到的问题：推理太慢

### 2019.11.8

- [x] 小图不外扩15%
- [x] 输出 错误信息

- [x] predict_with_pb_from_source_img.py 合并labels.txt与json
- [ ] 继续完成top3接口并调试
- [x] 大图尝试用tfrecord再跑

### 2019.11.7

- [x] 模型测试：MASK大图测试、MASK小图(原始标注框+统一方形标注框)测试、GLASS大图测试、GLASS小图(原始标注框+统一方形标注框)测试
- [x] 联合测试接口代码

### 2019.11.6

- [x] 优化模型融合评估代码
- [x] 由目标检测得出的结果来测试已经训练模型（暂无GPU测试）

### 2019.11.5

- [x] 数据预处理
- [x] 数据分析，检查
- [x] 评估模型融合可行性、整理详细数据

### 2019.11.4

- [x] 评估MASK模型融合可行性
- [x] 新数据预处理

### 2019.11.1

- [x] 标注框从长方形变为正方形（为了resize的时候不发生形变）
- [x] 在第一步的基础上，正方形扩大15%
- [x] 边界处理：如果缺陷在边缘的话，以图片的边界为极限处理。
- [ ] 整理重新标注的数据
- [ ] 遇到的问题：tf.train.batch 两次读取tfrecord的顺序 不一样
- [ ]  https://blog.csdn.net/u013555719/article/details/80459284 试试解决顺序问题
- [ ]  decode png jpg 要换一下

### 2019.10.29

- [ ] 学习怎么fine-tunning。学后可以吹。
- [x] 学习到怎么看tensorboard 的graph
- [x] 根据XML标注文本，在图片上框选出来，并存储

### 2019.10.28

- [x] 优化整理读取图片进行预测的代码
- [x] 了解怎么运用tensorflow-slim进行fine-tuning
- [x] 完成inception V1-V3的总结

### 2019.10.28

- [x] 尝试将源图片concat成一个batch

### 2019.10.25

- [x] tf.global_variables_initializer()与 tf.local_variables_initializer()的区别，并记录笔记
- [x] tf.train.shuffle_batch与tf.train.batch的区别
- [x] 通过label = tf.stack(tf.one_hot(label - 1, NCLASS))学习tf.onehot
- [x] with slim.queues.QueueRunners(sess):

### 2019.10.24

- [x] 完成pb的加载进行预测


### 2019.10.23

- [x] 整理我写过的小脚本。tfrecord/sava  as /load pb/ load checkpoint/moire_crop/

### 2019.10.22

- [x] 完成pretty table
- [x] 知道List长度的话，append和直接Index赋值，哪个效率高？

### 2019.10.21

- [x] 初步完成调用pb与checkpoint进行孔分类预测的脚本

### 2019.10.18

- [x] 首要解决问题：为什么ckpt和pb都是只归成两类？？？？？？

  灵感：[models](https://github.com/tensorflow/models)/[research](https://github.com/tensorflow/models/tree/master/research)/[slim](https://github.com/tensorflow/models/tree/master/research/slim)/[preprocessing](https://github.com/tensorflow/models/tree/master/research/slim/preprocessing)/inception_preprocessing.py/preprocess_image

- [x] 整理build_image_data.py 的使用方法（to tfrecord）

- [x] 整理遇到的问题,训练时候用的GPU训练，验证的时候也要用GPU。

- [x] 发现文件名原来可以用中文的冒号，但 是英文的冒号不可以用

- [ ] 整理sess.run(output,{input:img})，怎么查看output和Input 这两个tensor的名称。而且，加载ckpt与加载pd文件input\output    tensor的很名称是不同的

- [x] 搞明白InceptionResnetV2/Conv2d_1a_3x3/weights/Initializer/random_uniform/shape:0什么意思？0是用来做什么的。没有0会怎样

- [x] 搞明白get_tensor_by_name 与get_operation_by_name 

- [x] 整理tensorflow命令行参数

- [x] 怀疑是网络的固化，要求塞进64\*299*299\*3的数据 

- [x] 遇到的难点：不知道往哪里个节点塞数据

- [ ] 感慨还是pb简单，输入是input/input:0 不用找节点

- [x] 转换训练更久的ckpt为pb，并验证看效果。（20000步的时候，抽取15个no_hole，识别成small_hole）

- [ ] ### 2019.10.17

- [ ] 复习，重新做编程题目二题

- [ ] SE_ResNeXt源码\论文研读，刚看完网络架构

### 2019.10.16

- [x] 制作完的tfrecord怎么用？
- [x] 了解使用TF-Slim fune tune模型的流程
- [x] 完成https://github.com/tensorflow/models/tree/master/research/slim中Exporting the Inference Graph中的export_inference_graph.py
- [ ] 复习，重新做编程题目一题

### 2019.10.15

- [x] SE_inception-Net源码研读，刚看完网络架构
- [x] 设想制作自己的分类训练数据集，github上找到源码，知道怎么从image生成tfrecord，从tfrecord反生成image.

### 2019.10.14

- [x] SE-Net源码研读
- [x] 复习，重新做编程题目三题

### 2019.10.12

- [x] 了解孔缺陷及二分类，哪两类？
- [x] SE-Net学习
- [x] 编程题目
- [x] 列表推导加入笔记
- [x] yield重新学习
- [x] 递归思想

### 2019.10.11

- [x] 了解ADC业务
- [x] 报告PPT
- [x] inception中不同卷积核过后，是怎么拼接在一起的？
- [x] range和xrange加入笔记

### 2019.10.10

- [x] 通过运行代码仔细研究ADC项目分类网络xception_train.py
- [x] 研究PyTorch 的 backward 为什么有一个 grad_variables 参数
- [x] 测试keras.application中的ResNet50模型
- [x] 重新写一遍反转链表的代码

### 2019.10.09

- [x] 大致阅读ADC项目分类网络xception_train.py
- [x] MobileNet了解
- [x] 了解Keras.application的应用

### 2019.10.08

- [x] F:\TCL\other_material\Dataset\ADC\adc4350v1\trainA检查到第6691张
- [x] 编程题目三题

### 2019.9.30

- [x] 改进cut.py成遍历多个文件夹，自动创建文件夹
- [ ] InfoGAN看论文、代码运行
- [ ] styleGAN论文

### 2019.9.27

- [x] 摩尔数据集处理
- [x] crop新ADC数据集
- [x] 编程题目

### 2019.9.26

- [x] 编程题目
- [x] 摩尔纹数据crop

### 2019.9.25

- [x] CGAN结合代码看论文
- [x] pytorch简单入门
- [x] 阅读pytorch-CycleGAN-and-pix2pix代码
- [x] 编程题目

### 2019.9.24

- [x] DCGAN结合代码看论文
- [x] 阅读pytorch-CycleGAN-and-pix2pix代码
- [x] 编程题目

### 2019.9.23

- [x] WGAN结合代码看论文

### 2019.9.20

- [x] 结合代码理解Semi-supervised GAN,并在jupyter notebook上进行训练
- [x] 结合keras代码重新温习original GAN,并在jupyter notebook上进行训练
- [x] 在jupyter notebook上训练WGAN
- [x] 剑指offer编程题目一题

### 2019.9.18

- [x] 数据清洗
- [x] 剑指offer编程题目

### 2019.9.17

- [x] CycleGAN-Tensorflow/data/ADC/adc_crop/下清洗trainA_all、trainB中的5倍图，并重新上传
- [x] ADC补充数据清洗不正常的数据，并crop

### 2019.9.12

- [x] CycleGAN代码结合论文总结下

### 2019.9.11

- [x] 去掉ADC模糊的照片，自己训练。

### 2019.9.9

- [x] PPT
- [x] 工作临时交接，关注CycleGAN-Tensorflow/checkpoints/20190903-0926_1171adc_crop/中events的大小，太大的时候kill -9 [PID]杀掉进程，重新运行CycleGAN-Tensorflow/run2Train.sh 

### 2019.9.6

- [x] 浏览ADC各个不同分类的数据
- [x] Generative Adversarial Networks_A Survey and Taxonomy论文读完
- [x] GANS-PPT制作

### 2019.9.4

- [x] 完成彩色区域的选取，但是还没有实现保存，以及Python的argpasser，效率的评估，参数的评估与优化(黑白阈值的设定对更多的数据集能通用吗?还不知道！)，代码的整理

### 2019.9.4

- [x] 初步实现图片黑白的跳变寻找
- [x] Generative Adversarial Networks_A Survey and Taxonomy论文研读，PPT制作

### 2019.9.3

- [x] Generative Adversarial Networks_A Survey and Taxonomy论文研读


### 2019.9.2

- [x] D\G tensorboard中损失图的解读
- [x] 编程题目：字符串中替换空格为另一字符串
- [x] 跑DCGAN代码，并修改超参数learning rate 试验效果

### 2019.8.29

问题：

- [x]    总结与工作汇报PPT制作

### 2019.8.28

- [x] GAN-Overview-Chinese论文看完
- [x] 总结SVM-LOSS及梯度
- [x] numpy-广播
- [x] 完成CS231n-softmax的代码作业

### 2019.8.27

- [x] 代码实现：用Pillow批量去除在图片上影响训练效果的无关文本
- [x] 数学建模题目及相关资料的阅读
- [x] GAN-Overview-Chinese论文阅读

### 2019.8.26

- [x] 目标：各种GAN的结构及Loss function整理成PPT（大致完成）

### 2019.8.23

- [x] GAN paper的理论部分，并总结制作PPT
- [x] 完成Generative Adversarial Networks: An Overview论文阅读

### 2019.8.22

- [x] 完成CS231n-SVM的代码作业
- [x] Generative Adversarial Networks: An Overview论文阅读（未阅读完全）
- [x] 李宏毅深度学习课程-GAN,似乎懂了一些GAN paper的理论

### 2019.8.21

- [x] 完成CS231n-KNN的代码作业：代码层面理解KNN的原理；在作业的过程中，熟悉了numpy的应用，体会到了二层循环运算、一层循环运算、无循环的vectorized运算的神奇之处。
- [x] 《tensorflow实战》第五章：复杂的卷积神经网络
- [x] 计算机视觉识别：优化
- [x] numpy-从数值范围创建数组

### 2019.8.20

TODO

- [x] 计算机视觉识别-损失函数：原理层面理解KNN及SVM、Softmax损失函数
- [x] 启动numpy学习
- [x] 多层感知机（带有隐藏层的网络）
- [x] 《tensorflow》实战第五章：卷积神经网络
- [x] 学习jupyter notebook的简单使用

DO

- [x] 收获W的发音原来是double U

### 2019.8.19

DO

- [x] 斯坦福李飞飞计算机视觉识别：K近邻算法、线性分类
- [x] 训练集、验证集、测试集的区别
- [x] tensorflow实现手写数字 
- [x] 支持向量机的直观理解
- [x] 自编码器了解

### 2019.8.15

TODO

- [x] create_discriminator函数中第一层网络的rectified可视化出来 。可视化不出来的，因为通道有64
- [x] 搞清楚为什么要rectified，为什么要normalized?
- [x] 梯度消失是什么？
- [x] 反向传播算法

### 2019.8.14

问题

- [ ] 项目目标是什么？

TODO

- [x] Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks论文阅读

- [x] Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks代码阅读，不大懂

- [x] 了解残差块

- [x] 卷积层的输入输出层数的关系计算，总参数的计算

  

### 2019.8.13

问题：

- [ ] input = tf.concat([discrim_inputs, discrim_targets], axis=3) 通道拼接怎么理解，为什么discriminator的构建，将输入和输出拼接，卷积，激活，卷积，激活后就能输出判别的概率 ？
- [x] 1个epoch表示过了1遍训练集中的所有样本，那么为什么训练的时候要重复几千个epoch呢？

TODO:

- [x] 阅读CycleGAN, a Master of Steganography论文
- [x] 看CycleGAN-tensorflow代码

### 2019.8.12

TODO

- [x] 学习应用tools里面的脚本文件

问题：

- [x] 为什么discriminator_loss会先下降后上升，而generator_loss会先上升后下降？

- [ ] 摩尔纹的两个test文件，407的区别，407是什么意思呢？
- [ ] 训练时候，xshell断开连接，训练会继续吗？中断训练用ctrl+c 就可以了吧？
- [ ] 如果其中一个人在使用gpu进行训练 ，另一个人也使用相同的GPU,会发生什么呢？会干扰到第一个人的训练吗？

### 2019.8.9

问题：

- [x] discriminator的最后一层网络,卷积后sigmoid，直观上生成的图像每个点的代表的是什么呢？是分辩真假后的概率吗？看代码不像是，从代码上看，为什么create_discriminator这个函数能作为辨别器呢?
- [x] 为什么create_discriminator\create_generator代码这么写就能成为生成器和分辨器？
- [x] 代码中的损失和论文中的对不上。代码中gen_loss_GAN和discrim_loss对应论文中哪里呢？

TODO:

- [x] 结合Pix2pix工程，tensorboard学习
- [x] 进一步学习代码中的网络部分
- [x] 论文
- [x] 测试pix2pix中的其他数据集
- [x] which only penalizes structure at the scale of image patches？看论文Structured losses for image modeling部分（SSIM metric ......）

### 2019.8.8

- [x] 回头看pix2pix的论文，并结合代码理解论文
- [x] 卷积理论结合函数tf.layers.conv2d
- [x] 传统机器学习与深度学习的区别是什么？
- [ ] 服务器上的训练文件的名称的参数都是什么意思 ，ssim是衡量两张图片的相似度，psnr什么意思 
- [x] 感受野概念的理解

### 2019.8.7

TODO

- [x] 搭建好xshell 与winscp服务器配置
- [x] 搭建Xshell隧道（待跑训练，输出tensorboad测试）
- [x] 测试杨工的训练模型
- [x] 看pix2pix网络代码

问题：

- [x] server中的各文件是用来做什么的？用作Docker的
- [x] lab_colorization 是用来做什么的？
- [ ] batch size可以理解成batch张图片吗？batch的大小对训练速度的影响大吗？

我学了什么？

1. 熟悉了pix2pix工程，大概知道数据集该怎么处理，工程该怎么运行
2. 了解 GAN、CycleGAN、pix2pix、pix2pixHD、U-Net
3. 测试工程的各个输入参数

### 2019.8.2

- [x] 运行pix2pix-tensorflow工程中的测试部分
- [x] 了解pix2pix-tensorflow工程中各脚本文件
- [x] 训练的方向AtoB\BtoA是什么意思 ？
- [x] 数据集中的test、train、val各是什么意思？我理解的val是输入，但是为什么输入也是一对匹配的图像？因为test的输出是三张图片:input/output/target/，输入的匹配图像是Input和target,程序运行的结果 是output
- [x] process.py 中blank操作，将原图像添加一个白色正方形是为了做什么？
- [x] export这种mode是用来做什么的？

### 2019.8.1

- [x] GAN的初步了解
- [x] pix2pix论文的初步阅读
- [x] 神经网络的基础学习

### 2019.7.2

- [ ] 位运算实现两个整数的加法运算

### 2019.6.18

- [ ] 位运算实现两个整数的加法运算

### 2019.6.15

- [ ] 《Intermediate Python》容器，枚举部分
- [ ] 数值分析向量范数与第6章

### 2019.6.13

- [x] 看论文Learning to Reconstruct People in Clothing from a Single RGB Camera，和以前有什么不同？
- [ ] 《Intermediate Python》容器，枚举部分

### 2019.6.12

- [x] C++题目6.1,6.2
- [x] 《Intermediate Python》P47 - 53
- [ ] 《SLAM十四讲》复习第六讲，及 1 - 5 章漏掉的
- [x] 数值分析复习第5章节至少开头
- [x] 思考工程如何创新，如何进展 

### 2019.6.11

- [x] 数值分析复习

- [x] C++ 题目 5.6 - 5.8

- [x] 《SLAM十四讲》复习 1 - 5 章

- [x] 看Skeleton Tracker 代码，搞清楚SMPL是不是一定要与相对应的骨架绑定在一起

- [x] 《Intermediate Python》装饰器

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
     
  
  
  
  
  
  
  
  
  
  ​	