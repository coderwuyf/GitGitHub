### 多人协作--Git部分

#### 一、克隆仓库到本地

以组员的身份克隆work仓库到本地

```git
$ git clone https://github.com/JW04191215/work.git
```

#### 二、完成任务并推送到自己的仓库

要完成组长仓库的issue，每个issue都会生成一个编号

如图

![](https://doc.shiyanlou.com/document-uid310176labid9824timestamp1548757134030.png/wm)

现在，首先完成1号 issue

创建文件，添加到暂存区，提交，查看本地仓库分支状态：

```git
$ echo 'hello! world!' > one.txt//创建一个文本文件
$ git add .//添加到暂存区
$ git commit -m 'fix #1 addFile one.txt'//提交到版本区
$ git push
```

注意在执行 commit 命令时，备注信息里有个 “fix #1”，这是必要的，当备注信息中含有此字样的 commit 出现在组长仓库，仓库中编号为 #1 的 issue 就会自动关闭。类似的字样还有 “fixes #xxx、fixed #xxx、closes #xxx、close #xxx、closed #xxx”，这些并不重要，选择字母最少的 fix 就可以了。当然偶尔忘记写这个字样也不要紧的，issue 可以手动关闭，甚至关掉的 issue 还能再开。



#### 三、提PR&检查合并PR



PR：pull request,字面意思：‘拉，请求’，即允许被拉取的请求

这步操作的目的是：把组员的仓库添加到组长的仓库



实际操作：

首先，从组员的work仓库master分支给组长的work仓库master分支提一个PR

![](https://doc.shiyanlou.com/document-uid310176labid9824timestamp1548757171365.png/wm)



如下图所示，仔细检查紫色框中的内容是否正确，再看绿色椭圆形框中的绿色字样 “Able to merge.”，说明这个 PR 中的修改跟目标分支没有冲突：

![](https://doc.shiyanlou.com/document-uid310176labid9824timestamp1548757180192.png/wm)

左侧红框为组长仓库，右侧红框为组员仓库

然后点击创建PR，跳转到确认页面，最后点击确认，即可完成本次提交PR工作，并且自动跳转到组长的work仓库的PR的merge合并页面：

![](https://doc.shiyanlou.com/document-uid310176labid9824timestamp1548757219135.png/wm)

该页面只有参与项目协作的成员有权限进入，当前 GitHub 的登录用户是组员，所以可见，且对这个仓库有完全的管理权限，除了删除仓库。



重要的一点：提了 PR 之后，一定要求参与项目的其他成员来检查合并，不要自己来，尽管自己有权限。



上图中绿色按钮是个下拉按钮，合并 PR 的方法有三种，分别解释一下：



`Create a merge commit` ：这种方式会在组长仓库的 master 分支上生成一个新的提交，且保留 PR 中的所有提交信息。这是一种常规操作，用得最多。



`Squash and merge` ：压缩合并，它会把 PR 中的全部提交压缩成一个。此方法的优点就是让提交列表特别整洁。一个 PR 里有很多提交，每个提交都是很细小的改动，保留这些提交没什么意义，这种情况就使用此方法合并 PR。



`Rebase and merge` ：这种方法不会生成新的提交，例如 PR 中有 6 个提交，用此方法合并后，组长仓库也会新增 6 个提交。注意，这些提交的版本号与组员的提交不同，此外完全一样。



此处使用第一种方法合并



切换到组长账号，跳转到PR页面，使用第一种方法合并

![](https://doc.shiyanlou.com/document-uid310176labid9824timestamp1548757232986.png/wm)



这种合并的结果，生成了一个新的提交

同时，issue由两个变成了一个，在和并PR之后，#1 issue被关闭了

**需要注意的一点：从 A 向 B 提 PR 后，在 PR 合并或关闭前，A 上所有新增的提交都会出现在 PR 里。**



#### 四、同步主仓库

组员要随时保持自己的master分支与组长的一直，以避免在下次提PR时出现冲突。组长的仓库就是主仓库

```git
//首先，使用remote系列命令来增加一个关联主机
//执行git remote add [主机名] [主仓库的地址]，注意，主仓库的地址使用 https 开头的
//主机名可以任意，除origin以外就可以
//因为
// git push origin master等同于
// git push git@github.com:Acmen5/work master

$ git remote add up https://github.com/Acmen5/work

/*---------------------------------------------------*/

//同步主仓库的方法：
//一、
$ git pull --rebase up master
//二、
$ git rebase up/stream

//git pull --rebase = git fetch + git rebase
```



