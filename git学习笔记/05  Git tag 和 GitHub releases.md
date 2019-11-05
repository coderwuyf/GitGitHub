### 一、Git标签的作用

> 使用场景:
>
> 1. 在一个项目中，我们可能需要阶段性地发布一个版本，比如 `V1.0`、`V1.0.2`、`V3.2 Beta` 之类的
> 2. 在一个长期大型项目中，可能会有数千个提交版本，我们可能需要对重要的节点性提交打个记号
> 3. 在一些项目相关的书籍中，我们会看到 “执行 xxx 命令签出这个版本以查看对应的代码”

#### 1.1 创建标签

与  在仓库被 Fock 时 issue 不会跟随相似，Tags 通常在本地使用git命令创建后推送到GitHub后，Tags 只存在与项目仓库内，Fock 或提 PR 都不会带上Tags。

在多人协作项目中，通常由组长对主仓库设置 Tags 



操作如下：

1. 克隆仓库、配置信息、查看提交版本历史

```git
$ git clone httos://github.com/Acmen5/work.git
$ git log --oneline | nl 
```

2. 创建标签

```git
//注意： 我们创建标签是给具体的某次提交创建标签，与分支无关

//指令格式为：
//$ git tag [标签名] -m [备注信息] [提交版本号]
//虽然-m [备注信息] 可以省略，但建议保留，便于使用者阅读

//执行代码：
$ git tag v1.0 -m '发布项目正式版本 1.0'
```

#### 1.2 查看标签

```git
// 1.执行 git tag 命令显示仓库中的全部标签列表
// 2.执行 git show [标签名] 查看标签详细信息

$ git tag
$ git show v1.0 | cat
```

#### 1.3 删除本地标签

```git
// 每当创建一个本地标签之后，在仓库的主目录的.git/refs/tags目录下就会生成一个标签文件

$ tree .git/ref/tags

// 删除标签的命令为: git tag -d [标签名]
// 执行命令如下：

$ git tag -d v1.0

```

#### 1.4 将本地标签推送到远程仓库

```git
// 首先，对前两个提交版本创建对应标签
/*
e07fae8 Initial commit
4f07e76 Create README.md
052ec04 fix #1 addFile one.txt
df01d49 Merge pull request #3 from JW04191215/master
*/
$ git tag 001 -m 'the first tag' e07fae8
$ git tag 002 -m 'the secone tag'


// 执行 git push origin [标签名] 推送标签到远程仓库
$ git push origin 001
// 执行 git push origin --tags 可将全部的本地标签推送到远程仓库 
```

#### 1.5 删除远程仓库标签

```git
// 对应指令为：
// $ git push origin :refs/tags/[标签名]，标签名也就是文件名

$ git push origin :refs/tags/001

//注意： 这步操作只是删除了远程仓库的标签，并没有删除本地标签，因此，本地标签需要自己手动删除
```

#### 1.6 签出版本

​	现在介绍一下关于 “签出版本” 的操作，

我们会见到类似这种说明：

​	“如果你从 GitHub 上克隆了这个程序的仓库，那么可以在仓库主目录下执行 git checkout xxx 签出程序的这个版本。” 

​	其实**签出版本**就是指定**某个提交版本**创建一个**新的分支**。

```git
//假定当前的 work 仓库就是一个程序，我们要签出 001 版本，执行以下步骤即可。

//首先，执行 git checkout [标签名] 切换到之前的某个提交版本，然后执行 git checkout -b [新的分支名]
$ git checkout 001 //切换到001标签
$ git checkout -b 001 //创建一个001的新的本地分支branch

//这样就完成了提交版本的签出工作
```

### 二、GitHub 的 releases

GitHub 的 releases 是 2013 年发布的新功能，旨在协助软件开发者分发新版本给用户，关于这个功能这里仅作简单介绍。

当项目组织宣布发布一个软件产品的版本，发布过程就是一个将软件交付给最终用户的工作流。版本是具有修改日志和二进制文件的一类对象，它们提供了 Git 工作流之外的完整项目历史，它们也可以从存储库的主页上被访问。发布版 release 附带发布说明和下载软件或源代码的链接。按照许多 Git 项目的约定，发布版本与 Git 的标签 tag 绑定。您可以使用现有的标签，或者让 release 在发布时创建标签。这就是上面查看 GitHub 仓库中标签信息时出现的场景。

标签是 Git 中的概念，而 releases 则是 Github、码云等源码托管商所提供的更高层的概念。Git 本身是没有 releases 这个概念，只有 tag。两者之间的关系则是，release 基于 tag，为 tag 添加更丰富的信息，一般是编译好的文件。