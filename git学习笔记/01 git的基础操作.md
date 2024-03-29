## 一、Git仓库的三大区域

Git本地仓库有三大区域：工作区、暂存区、版本区

![](https://doc.shiyanlou.com/document-uid310176labid9805timestamp1548755776759.png/wm)



## 二、一次完整的修改、提交、推送

首先，进入仓库主目录（我的仓库目录为：D:\Git\learnGit\-Git-GitHub）

1. 查看仓库状态

```git
$ git status
//提示为 No commits yet
//nothing to commit
```

2. 对工作区进行修改

```git
//首先在仓库目录创建一个新的文件
$ echo 'hello world' > one.txt
//然后通过git status再次查看仓库状态
$ git status
//提示为：
//No commits yet
//Untracked files(未跟踪文件)： one txt
```

3.  添加修改到暂存区以及撤销修改

```git
//$ git add [文件名]命令，跟踪此新建文件，也就是把文件添加进暂存区，准备提交
$ git add one.txt
//再次通过git status查看仓库状态
$ git status
//提示为：
//No commits yet
//Changes to be committed(要提交的变更)
//		new file:	one.txt

/*--------------------------------------------------*/

//如果对多个文件进行或目录进行增删改，可以使用
//$ git add . 命令全部添加到缓存区

/*--------------------------------------------------*/

//撤销修改，使用
//$ git reset -- [文件名]或git rm -- cached [文件名]
//以上命令即可，即将目标文件从暂存区移除
//如果需要撤销暂存区所有文件，使用
//$ git reset -- 

/*---------------------------------------------------*/

//$ git diff 命令：可以用来查看工作区被追踪的文件的修改详情。注意：只有版本区中存在的文件才是被追中的文件
//首先修改README.md文件，然后再执行git diff命令
//就可以查看到文件修改的详情了
```



4. 查看提交历史

```
//使用$ git log命令可以查看版本区提交的历史记录
关于查看提交历史记录的命令，有些常用的选项介绍一下：

/*
	git log [分支名] 查看某分支的提交历史，不写分支名查看当前所在分支
	git log --oneline 一行显示提交历史
	git log -n 其中 n 是数字，查看最近 n 个提交
	git log --author [贡献者名字] 查看指定贡献者的提交记录
	git log --graph 图示法显示提交历史
*/
```

5. 配置个人信息

```git
//在使用git commit命令提交之前，还需要对本地配置进行确认
//对git进行本地配置
//配置user.email与user.name
$ git config --global user.email "[GitHub账号邮箱]"
$ git config --global user.name "[GitHub账号名字]"
//可以使用$ git config -l 查看git的本地配置
```

6. 提交暂存区的修改

```
//通过$ git commit 命令生成一个新的提交，在这条命令后面需要加上-m [文本],由于作为提交的备注
$ git conmit -m "修改了README文件"

/*---------------------------------------------------*/

//在提交完成之后，暂存区的修改会被清空
//然后通过git log命令查看提交的历史记录
$ git log
//此时得到的结果中 每个commit后面的十六进制序列号就是提交的版本号，每个提交都有单独的版本号
//当然此时得到的结果是按提交时间倒序排列的，也就是最后一次提交将显示在最开始的位置，此时我们可以使用
//$ git log --reverse命令将顺序翻转

/*---------------------------------------------------*/

//在此介绍一条挺实用的git命令
//$ git branch -avv
//作用：用来查看全部分支信息

```



## 三、版本回退

如果发现提交的文件内容有误，该如何操作？

方法有两种：

(1)修改本地文件，然后添加到暂存区，提交、推送

(2)撤销最近一次提交，修改文件、重新提交、推送

第一个方法与前面的操作完全相同，因此此处省略

方法二中的版本回退操作演示如下：

```git
$ git reset --soft HEAD~1
//这条指令的作用是：撤销最近一次提交，将修改还原到暂存区。 --soft便是软退回，对应的还有--hard硬退回。此外HEAD^表示撤销一次提交，HEAD^^表示撤销两次提交，HEAD~n表示撤销n次提交

//在执行完此步骤之后，上一次提交的所有数据将全部还原到暂存区，此时使用git branch -avv时，将提示本地分支master已落后于远程分支origin/master一个提交

//执行git status查看仓库状态，将发现上一个提交一全部还原到暂存区

//到此则表明，版本回退完成了

//然后修改文件，重新提交
$ git add .
$ git commit -m "reset README file"

```



## 四、处理commit时间线分叉

```git
//在处理完版本回退操作，以及重新提交的操作之后
//此时执行以下两条指令
$ git status
$ git branch -avv
//发现此时本地仓库master与远程仓库origin/master分支在提交版本上出现了冲突，又叫做提交时间线分叉
//此时将无法使用 git push指令完成推送操作

//解决办法如下：
$ git push -f
//-f选项是--force的缩写，即强制推送
```



## 五、本地commit变化记录

假如出现这样一种情况，发现上一次的版本回退的操作是误操作，想回到上上次或者之前的任何一个版本，该怎么办？

此时我们就可以使用到一下指令了

```git
$ git reflog
//这条指令的作用：用于记录本地仓库每一次版本变化
//由于reflog记录只存在于本地仓库中，只要本地仓库没有被删除，就可以回退到任何版本位置。当然本地仓库被删除之后，记录也会跟着消失
```

