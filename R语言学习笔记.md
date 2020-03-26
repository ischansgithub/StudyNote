#R语言学习笔记
---
##列表
列表可以包罗万象：列表是包含不同类型的元素的R对象，如数字，字符串，向量，以及列表中也可包含另一个列表。 列表还可以包含矩阵或函数作为其元素。

```
list_data <- list("Red", "Green", c(21,32,11), TRUE, 51.23, 119.1);
```

##平均值mean()
mean(x, trim = 0, na.rm = FALSE, ...)

- x  是输入向量。
- trim：0-0.5  用于从排序的向量的两端删除一些观测值。
- na.rm - 用于从输入向量中删除缺少的值。

##中位数median()
median(x, na.rm = FALSE)

- x  是输入向量。

##sample()

e.g  试产生5组自助样本

`
X=(0:9)  
mat = matrix(0, nrow = 5, ncol = 10) 

for(i in 1:5){
    mat[i,] = sample(X,10,replace = TRUE)
}

mat
` 
##cbind()/rbind()
cbind： 根据列进行合并，即叠加所有列，m列的矩阵与n列的矩阵cbind()最后变成m+n列，合并前提：cbind(a, c)中矩阵a、c的行数必需相符

rbind： 根据行进行合并，就是行的叠加，m行的矩阵与n行的矩阵rbind()最后变成m+n行，合并前提：rbind(a, c)中矩阵a、c的列数必需相符


##runif（）
runif（）:R语言生成均匀分布随机数的函数

句法是：runif(n,min=0,max=1)    n表示生成的随机数数量，min表示均匀分布的下限，max表示均匀分布的上限；若省略参数min、max,则默认生成[0,1]上的均匀分布随机数。

##combn（）
combn(x, m) ：
从1至x中取m个数，排列出所有组合。
