====================================================================================================================================================================================================
viz.showWidget("cube", cube, params.volume_pose);  ->
p.volume_pose = Affine3f().translate(Vec3f(-p.volume_size[0]/2, -p.volume_size[1]/2, 0.5f)); ->

note:  a.translate(t) is equivalent to Affine(E, t) * a, where E is an identity matrix ,so volume_pose is equivalent to  [ E * Vec3f(-p.volume_size[0]/2, -p.volume_size[1]/2, 0.5f) ]

====================================================================================================================================================================================================

在DynamicFusionApp这个结构体中，构造函数设置了默认参数，用其中的一些默认定义了一个立方体并显示出来。


看到在DynamicFusionApp这个结构体中的构造函数
====================================================================================================================================================================================================
有关ICP迭代：

KinFuParms类中有成员变量vector<int> icp_iter_num
KinFuParms类中有成员函数default_params_dynamicfusion,在这个函数中对icp_iter_num赋值：icp_iter_num.assign(iters, iters + levels)
而其中的iter由局部变量生成：const int iters[] = {10, 5, 4, 0};const int levels = sizeof(iters)/sizeof(iters[0]);

在ProjectiveICP类中有成员变量std::vector<int> iters_;
iters_由ProjectiveICP类中的成员函数setIterationsNum。
在KinFu类的默认构造函数中通过传入KinFuParms类中的成员变量icp_iter_num设定了ProjectiveICP类中的成员变量iters_ ：iters_icp_->setIterationsNum(params_.icp_iter_num);

总结：iters_ ＝ icp_iter_num ＝ 局部变量const int iters[] = {10, 5, 4, 0}  （前提是，局部变量iters的维度为4）
====================================================================================================================================================================================================



p.volume_size = Vec3f::all(3.f);
p.volume_pose = Affine3f().translate(Vec3f(-p.volume_size[0]/2, -p.volume_size[1]/2, 0.5f));
