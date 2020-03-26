# GAN学习笔记

### 什么是latent space?

### ENCODER-DECODER中的decoder与GAN中的discriminator什么区别？

### 极大似然估计

### 为什么是MIN_G  MAX_D V(G,D)?

### 为什么generator的loss“gen_loss_GAN = tf.reduce_mean(-tf.log(predict_fake + EPS))” 没有 log(predict_real)项？而且不是tf.log(1-predcict_fake)?

更容易训练

### 为什么在实际中训练generator的次数比训练discriminator少？