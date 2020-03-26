# VideoBasedReconstruction

## overview

   We first estimate an initial body shape and 3D pose at each frame by fitting the SMPL model to 2D detections.

   Given such fits, we associate every silhouette point in every frame to a 3D point in the body model

   then transform every projection ray according to the inverse deformation model of its corresponding 3D model point   

  After unposing the rays for all frames we obtain a visual hull that constrains the body shape in a canonical T-pose

​      (预先准备：分割出图片流中的人体：掩膜；运用CNN估计二维关节节点：2D-joint)

1. 使用2D-joint，调整SMPL模型更好地适应每帧图片中人体的pose and shape
2. 使用掩膜，求解剪影相机的光线,找出剪影的每一个点在3D模型中对应的点，F帧的A-pose对应到一个T-pose模型中。接着进行“使光线与模型轮廓的距离最小”、“拉普拉斯算法：平滑形变点”、“重构顶点的偏差”、“人体对称优化”
4. 根据mask生成蒙皮

## some key points

- the set of rays going from the camera to the silhouette points define a constraint cone,从相机到轮廓点的一组光线定义了一个约束圆锥体
- 用Pose Reconstruction中估计的pose集合去解开定义的圆锥体
- 求解圆锥体的方法 removes blend-shape calculations from the optimization problem and significantly reduces the memory foot-print of the method，Without unposing the vertex operations and the respective Jacobians would have to be computed for every frame at every update of the shape
- numpy一定要1.13.0版本才能运行？换成1.16.3就会报错
- '"Automatic Estimation of 3D Human Pose and Shape from a Single Image" : 用CNN估计二维关节，定义一个目标函数并直接优化姿势和形状，使三维模型的投影关节接近CNN估计的二维关节。
- what do pose priors used for?  当joint的置信值很低时，要用到姿势先验
- Most methods for 3D pose estimation use some sort of pose prior to favor
  probable poses over improbable ones大多数三维姿态估计方法都是先使用某种姿态先验，然后再使用可能的姿态，而不是不可能的姿态。
- b2m.shape(11, 1)    beta.shape(10, )     pose.shape(72, )    keypoints.shape(frames, 18*3)

## Pose Reconstruction

###        output of Pose Reconstruction

- The output of this step is a set of poses

$$
{\{θ p\} } _{p=1}^F
$$

​	    for the F frames in the sequence. 

​		F frames有F个poses，所以Pose Reconstruction就是估计总共F帧中包含的F个poses.每个poses包含两个东西：pose与translation。

```python
# 这是其中一帧的数据.pose参数是使用axis-angle来定义的,每个关节，用一个三维向量表示，24个关节，就有72个参数
('poses', array([ 3.26998234e+00,  1.51818730e-02,  1.70825962e-02, -5.21816872e-02,
        8.74096602e-02,  9.21730027e-02, -4.22959439e-02, -5.63149825e-02,
       -8.50844607e-02, -9.38752852e-03,  1.88195296e-02,  2.32643890e-03,
       -5.67885414e-02, -8.38157609e-02, -1.35809556e-02, -4.42727469e-02,
        5.57661615e-02,  1.29176341e-02,  1.96232013e-02, -2.83522792e-02,
        6.05870150e-02,  2.02538632e-03,  1.69661835e-01,  8.70693922e-02,
       -2.80794296e-02, -2.23564103e-01, -5.70936836e-02, -5.47182746e-02,
       -2.04401114e-03, -1.49759287e-02,  4.10332484e-03,  1.27222732e-01,
       -9.34187174e-02,  7.26592243e-02, -4.12170142e-02, -3.10214087e-02,
       -1.26772281e-02, -4.03809473e-02, -1.14283515e-02, -1.05259120e-01,
        1.02310181e-02, -1.53649941e-01, -1.19747944e-01,  5.71090244e-02,
        1.68980300e-01,  9.63110849e-02,  3.11933365e-02,  3.28049213e-02,
        8.72079432e-02, -1.82174146e-01, -6.53898239e-01,  1.65718079e-01,
        1.04244165e-01,  6.33859873e-01, -1.41568676e-01, -5.17173037e-02,
        1.00971527e-01, -2.55387098e-01,  8.13969001e-02, -1.60514534e-01,
        2.03833412e-02,  8.06308910e-02, -1.63729101e-01,  5.58873080e-02,
       -7.52141848e-02,  1.34833530e-01,  1.50084630e-01,  2.11157836e-02,
        1.42058030e-01,  1.01697192e-01, -9.63317044e-03, -9.01120454e-02],
      dtype=float32))
# camera translation  等价于 body translation
('trans', array([-2.3390874e-04,  2.8282195e-01,  2.4355714e+00], dtype=float32))

```



### 	《Automatic Estimation of 3D Human Pose and Shape from a Single Image》

- 这篇论文优化了SMPL模型参数，从而能够更好地适应从图像中获取到的2D joint detections

## Experiments

作者测试了两个公开数据集：with minimal clothing (MC) (DynamicFAUST [8]) and with clothing (BUFF [80])，并与KinectCap方法（[Detailed Full-Body Reconstructions of Moving People from Monocular RGB-D Sequences](http://www.researchgate.net/publication/300412573_Detailed_Full-Body_Reconstructions_of_Moving_People_from_Monocular_RGB-D_Sequences)）作了对比

## what  is chumpy?

- Chumpy is a Python-based framework designed to handle the auto-differentiation problem（用来计算导数）

- Chumpy always casts integers to floating-point, because it depends on values changing smoothly

- chumpy can optimize an objective and get derivatives(什么是derivatives?)

- Chumpy supports some interesting functions (svd, tensorinv, inv, lstsq)

- "We optimize Econs using a “dog-leg” trust region method using the chumpy autodifferentiation framework"，即在VideoBasedReconstruction中用来优化
  $$
  E _{cons} = E _{data} + w_{lp} E_{lp} + w_{var} E_{var} + w_{sym} E_{sym}
  $$
  

## 运行时间

- 对于一个25s的视频，有647帧，第一步pose_reconstruction的耗时为6小时15分钟。第二步consensus耗时4分钟。第三步在以每5帧为步长的条件下耗时2分钟

## 自己的优化方向

- pose reconstruction 使用CNN使训练更快。
- 融合骨架
- 将chumpy替换成tensorflow或者numpy
- 模型拿出来，与深度数据（或者RGB图片）融合，实现模型的实时运动
- 细看整个工程的关键算法，思考能否融合数值分析所学的内容

## 遇到的难点

- 制作camera.pkl时候的长宽是1080\*1080，程序mask.hdf5打开后print shape，发现'shape of mask :', (648, 1080, 1080)，648帧，长宽也是1080\*1080。但是输入的视频是每帧都是640\*640的呀。这就会导致以下源代码

  ```
  gaussian_pyramid(rn_m * dist_o * 100. + (1 - rn_m) * dist_i......)
  ```

  报错：ValueError: operands could not be broadcast together with shapes (540,540) (320,320) ，两个不同类型的矩阵相运算
  
  解决方法：细看源码，追根溯源。发现所谓的540是由1080×resize（默认为0.5）得到的。而1080是从camera.pkl中得到的，那么为什么我不将camera的参数改掉呢？所以自己用640×640参数重新生成 了camera.pkl
  
- 和 <https://github.com/thmoa/videoavatars/issues/15>  这个 ISSUE一样，

  报错：ValueError: attempt to get argmin of an empty sequence

  背景：由yoloct生成的Mask的颜色有alpha通道，所以掩膜有透明的颜色，因此灰度图在二值化人体部分并不是全白的，轮廓也不是那么分明。还有一点值得注意的是yoloct输出的带有掩膜的视频须要自己用脚本去分割成一帧帧的图片，但是不知道为什么最后一帧出现错误，因此相对于实际视频少了一帧。而Keypoints是正常的。

  解决方法：在yoloct工程中将掩膜的alpha值修改为1，也即不透明，并将掩膜颜色设为（255, 255, 255）白色。同时修改videoavatars工程中的prepare_data中的masks2hdf5.py脚本中cv2.threshold函数的阈值

  ```python
  cv2.threshold(silh, 110, 255, cv2.THRESH_BINARY)
  ```

  不知道和Keypoints与mask的frame对齐会不会有关系，这次特意将keypoints中最后一帧的关键点去掉，保证与mask对齐。
  
- body height 与 camera 内参对重建的影响大吗？

- 