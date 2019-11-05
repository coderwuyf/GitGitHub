## 一、添加SSH关联授权

由于在之前的操作中，每次push都需要手动输入用户名和密码，若想省略相关登录操作，可以在系统中创建SSH公私钥，并将公钥放到GitHub指定位置。

```git
//在任意指定文件夹目录下执行以下指令

$ ssh-keygen

//然后系统将会弹出几条提示信息，不用管内容，直接几次回车即可
//操作完成之后，系统将生成公钥与私钥两个文件

$ cat ~/.ssh/id_rsa.pub
//然后将公钥的内容复制
```

接下来就是在GitHub网页上添加公钥，操作步骤如下图：

![](https://doc.shiyanlou.com/document-uid310176labid9816timestamp1548756492545.png/wm)

Title 自定义，把剪切板中的内容粘贴到 Key 中，点击绿色按钮添加 SSH Key 即可：

![](https://doc.shiyanlou.com/document-uid310176labid9816timestamp1548756503765.png/wm)

接下来回到GitHub仓库目录，选择下载方式，将之前的https换成ssh，然后删除本地仓库，使用新连接重新克隆克隆仓库



```git
//在本地仓库的上一级目录
$ rd /s -Git-GitHub
//这条命令的作用为删除文件夹以及文件夹里的内容
$ git clone git@github.com:Acmen5/-Git-GitHub.git
//使用ssh的方法克隆远程仓库的内容到本地仓库
```



这一系列的操作的优点在于：

1. 免密码推送，即执行 `git push` 时不再需要输入用户名和密码了；
2. 提高数据传输速度



## 二、为git命令添加别名

我们可以为经常出现的几条命令设置一个较短的别名，比如 git status,或者 git branch -avv等经常使用的几条语句设置别名，使用起来更方便

```git
//设置别名的命令是 git config --global alias.[别名] [原命令]，如果原命令中不仅有指令还有选项，则需要加上引号
$ git config --global alias.st status
$ git config --global alias.br 'branch -avv'
$ git config --global alias.com 'commit -m'


/*---------------------------------------------------*/
//举例：git status的使用方法
$ git st
//git branch -avv的使用方法
$ git br
```

在此阶段学习的 过程中遇到的问题：

```
具体过程如下：
我在cmd命令行中输入：git congfig --global alias.br 'branch -avv'之后，
使用:git config -l查看配置信息时却出现了别名设置错误的情况，
显示的结果为：alias.br='branch
/*---------------------------------------------------*/
解决办法:
1.重新设置别名，系统会自动使用最后设置的别名
2.找到git根目录下的.gitconfig文件，然后使用文本工具打开，直接进行修改即可

/*---------------------------------------------------*/

需要注意的点：
cmd命令行中，在设置别名的时候，原命令需要使用双引号，不能使用单引号。若使用单引号，别名将会设置错误。
```



## 三、git分支管理

#### 3.1 git fetch 刷新本地分支信息、git pull 拉去远程分支到本地

使用情况：当在GitHub上直接修改内容并提交之后

```git
//此时输入 git fetch
$ git fetch
//可以看到，本地分支 master 的版本号无变化，而远程分支已经更新
// fecth命令的作用：刷新保存在本地仓库的远程分支信息

/*---------------------------------------------------*/

//此时若想本地master分支提交的版本为远程端的版本，就需要使用下面这条命令:
$ git pull
// pull命令的作用：拉取远程仓库的的数据到本地
```



#### 3.2 创建新的本地分支

举例说明：

一个项目上线1.0版本，而研发部门需要看法1.1,1.2两个测试版，增加不同的功能。测试版的代码显然不能将代码放到正式版1.0的分支中，

此时，就需要创建新的分支来保存不同版本的代码



步骤如下：

1. 首先克隆远程仓库到本地，进入仓库主目录，查看分支信息

```git
$ git clone git@github.com:Acmen5/GitGitHub.git
$ cd GitGitHub
//GitGitHub为仓库主目录
$ git br
//br 为 branch -avv的别名
```

2. 执行git branch [分支名]，创建新的分支

```git
$ git branch dev

/*---------------------------------------------------*/
// 显示结果：
/*
  dev                   892a1e4 Updata one.txt
* master                892a1e4 [origin/master] Updata one.txt
  remotes/origin/HEAD   -> origin/master
  remotes/origin/master 892a1e4 Updata one.txt
*/
//现在*号在master上

//此命令创建新的分支后并未切换到新的分支，仍然在master分支上

```

3. 执行 git chenkout [分支名]，切换到新分支

```git
$ git checkout dev

/*---------------------------------------------------*/
// 显示结果：
/*
* dev                   892a1e4 Updata one.txt
  master                892a1e4 [origin/master] Updata one.txt
  remotes/origin/HEAD   -> origin/master
  remotes/origin/master 892a1e4 Updata one.txt
*/
//现在*号在dev上

```

至此已经完成了创建本地分支的操作了，但是每次创建新分支还要手动切换太麻烦

简化以上操作：

```git
$ git checkout -b dev1
// 此命令的作用 : 创建分支并切换到新分支

/*---------------------------------------------------*/

/*
  dev                   892a1e4 Updata one.txt
* dev1                  892a1e4 Updata one.txt
  master                892a1e4 [origin/master] Updata one.txt
  remotes/origin/HEAD   -> origin/master
  remotes/origin/master 892a1e4 Updata one.txt
 */
// 现在*号出现在dev1上
```



新建的所有分支的版本号与主分支 master 一致，因为在哪个分支上创建新分支，新分支的提交记录就与哪个分支一致。



此时如果需要在分支dev1上开发一个新的功能，也就需要新建一个文件new_func1

```git
$ echo '新功能测试~~' > new_func1
//然后添加到暂存区，提交到版本区
$ git add .
$ git com '新功能测试'
//com 为 'commit -m'的别名

/*---------------------------------------------------*/

//此时查看分支状态
$ git br
//结果显示
/*

  dev                   892a1e4 Updata one.txt
* dev1                  045043f 新功能测试~~
  master                892a1e4 [origin/master] Updata one.txt
  remotes/origin/HEAD   -> origin/master
  remotes/origin/master 892a1e4 Updata one.txt
  
*/
```

#### 3.3 将新分支中的提交推送到远端仓库



执行git push [主机名] [本地分支名]:[远程分支名]，就可以将本地分支推送到远程仓库的分支中，通常冒号前后的分支名如果相同，则可以省略冒号和后面的远程分支名，如果不存在，会自动创建

```git
$ git push origin dev1:dev1
//可简写为 git push origin dev1

/*---------------------------------------------------*/
//此时我们查看分置状态
$ git br
可以看到远端分支 origin/dev1的信息已经在本地存在，且与本地分支一致
/*

* dev1                  045043f 新功能测试~~
  remotes/origin/dev1   045043f 新功能测试~~
  
*/
```



#### 3.4 本地分支跟踪远程分支



目前每次在dev1分支上修改并提交，推送到远程仓库都需要输入一长条命令，太麻烦，如何能像master分支一样直接使用git push命令就好了？

```git
//解决办法：
//$ git branch -u [主机名/远程分支名] [本地分支名]
$ git branch -u origin/dev1
//其中 -u 指的是 --set-upstream 的缩写
//作用：将本地分支与远程分支关联，或者说使本地分支跟踪远程分支。如果是设置当前所在分支跟踪远程分支，最后一个参数本地分支名可以省略不写


/*---------------------------------------------------*/

//此外，该如何撤销本地分支对远程分支的追踪呢？
//执行 git branch --unset-upstream [分支名]
$ git branch --unset-upstream dev1
//如果撤销当前所在的分支的跟踪，分支名可以省略不写
$ git branch --unset-upstream

/*---------------------------------------------------*/

//以上操作为: 先将本地分支推送到远程分支，是远程分支创建新分支，然后再使本地分支追踪远程分支
//该如何在推送的同时，自动追踪远程分支?
//在 push 的时候末尾加上 --set-upstream或其简写 -u即可
```



#### 3.5 删除远程分支



```git
//首先，如果需要删除指定的某个远程分支
//使用:
//$ git push [主机名] :[远程分支名]
//如果一次性删除多个，可以使用以下命令:
//$git push [主机名] :[远程分支名] :[远程分支名] :[远程分支名]
//以上命令的原理为：将空分支推送到远程分支，结果也就是远程分支被删除
//本地运行及结果:

$ git push origin :dev
//结果显示dev分支已经丢失(gone)

/*---------------------------------------------------*/

//另一个删除远程分支的命令为:
// $ git push [主机名] --delete [远程分支名]
//本地运行及结果

$ git push origin --delete dev1
//结果显示dev1分支也被成功删除
//此时github仓库中只有master分支

```



####  3.6 本地分支的更名与删除



说完远程分支的删除之后，接下来就是删除本地分支了

```git
//首先删除本地分支的命令为:
//$ git branch -D [分支名]
//若想删除多个则只需要将要删除的分支名罗列在命令后面即可

//在此之前，还需要使用到一个极少用到的命令:
//$ git branch -m [原分支名] [新分支名]
//这条指令的作用是：给本地分支改名，若果修改当前分支的名字，则可以省略原分支名
//执行：
$ git branch -m dev dev2


//介绍完改名操作之后就开始删除本地分支
$ git branch -D dev1 dev2
//注意，当前分支不能被删除
//因此，要执行这条指令，需要先切换到master分支上

//结果： 当前仓库分支状态中，仅存master分支
```





