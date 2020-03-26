# Overview

SMPLify:  system that takes a 2D image and produces a posed 3D mesh

# Keypoints

- 论文中提到的interpenetration是什么意思？"prevent incorrect poses".由于使用2D图像，缺少深度信息，body part 间会相互交涉，重叠，这种动作体现在3D模型中是不科学的。

- 怎么解决interpenetration呢？define an interpenetration error term that exploits the capsule approximation introduced in 3.1

   
  $$
  E _{sp} (θ; β) = ....
  $$
  

  ```python
  # 体现在代码上是用了collision_obj函数 
  f.collision_obj = collision_obj(f.smpl, regs)
  ```

  