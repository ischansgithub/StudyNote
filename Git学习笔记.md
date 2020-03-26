# Git学习笔记
---


-  初始化Git仓库	git init

-  添加文件到Git仓库	
git add <file>  可以反复使用，添加多个文件
git commit	-m "<write log here>"  一次提交多个add后的文件

-  工作区状态	git status

-  查看修改的内容	git diff

-  在历史版本之间穿梭	
```
/*HEAD表示当前版本，HEAD^表示上一个版本，以此类推*/

git reset --hard commit <commit_id or HEAD^>
```


-  查看历史版本的日志，其中包括版本号	git log

-  退回历史版本，又想要回到未来的版本，用git reflog 查看命令历史，从而可以查询到版本ID

-  修改了一个文件后，还没有add到暂存区，仍然处于在工作区，用git checkout -- file 可以对工作区的修改进行丢弃

-  错误提示：fatal: remote origin already exists.
解决办法：$ git remote rm origin

-  要关联一个远程库，使用命令 git remote add origin ......在github新建一个ropository后注意会有提示，按照提示输入指令就好。

-  关联后，使用命令 git push -u origin master 第一次推送master分支的所有内容；

-  此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；












