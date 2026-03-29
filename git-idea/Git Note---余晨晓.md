# Git Note---余晨晓

*Git,一种用C语言开发的分布式版本控制系统*。可安装在Linux、macOS、Windows、Raspberry Pi上。
## 学习心得
在此之前，我对Git一无所知。经过这段时间的学习，我感觉自己仅仅只是揭开了计算机这片广袤领域的微小一角。但就算是这小小的一角也让我受益匪浅：

    关于创建分支：git branch 和 git checkout让我明白，分支就是平行的时空。当我需要进行任何修改时，不再需要复制整个文件夹，只需轻轻创建一个新分支，就能在一个完全隔离的环境中“为所欲为”。
    解决冲突：虽然初次遇到“合并冲突”时有点慌，但解决后我意识到，冲突不是错误，而是沟通的桥梁。它让我学会了如何精细化管理代码，也让我明白计算机学习不是一蹴而就的。

我会继续加油，向着理想不断前进！
***
## 初步认识

| 集中式(Linux)                                                | 分布式(Git)                                                  |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| 版本库(书)集中存放在中央服务器(图书馆)。要先从"图书馆"获取最新版本(借出)，处理后再推送回图书馆 | 没有中央服务器，不需要联网，多人协作只需把各自修改推送给对方 |
| 必须联网                                                     | 安全性高                                                     |

```
集中式和分布式的区别是：
你的本地是否有完整的版本库历史。
假设SVN服务器没了，那你丢掉了所有历史信息，因为你的本地只有当前版本以及部分历史信息。
假设GitHub服务器没了，你不会丢掉任何git历史信息，因为你的本地有完整的版本库信息。你可以把本地的git库重新上传到另外的git服务商。
```
***
## 目录
- [基本操作](#基本操作)
  + [把文件添加到版本库](#把文件添加到版本库)
  + [遇到的问题和解决办法](#遇到的问题和解决办法)
- [Git原理](#Git原理)
  + [工作区和暂存区](#工作区和暂存区)
  + [基于文档](#基于文档)
    + [修改](#修改)
    + [撤销修改](#撤销修改)
    + [删除](#删除)
- [进阶功能](#进阶功能)
  + [远程仓库](z#远程仓库)
  + [先有本地库，后有远程库时，如何关联远程库?](#先有本地库，后有远程库时，如何关联远程库?)
  + [若从零开发，该怎么实现文件互传?](#若从零开发，该怎么实现文件互传?)
- [分支管理](#分支管理)
  + [创建新的分支](#创建新的分支)
  + [合并分支到主分支](#合并分支到主分支)

+ [switch](#switch)
  + [回忆](#回忆)
***
## 基本操作
### 把文件添加到版本库

1. 创建空目录

   ```
   $ mkdir hello
   $ cd hello
   $ pwd
   /Users/User/hello
   ```

   `pwd`显示当前目录

2. 把目录变成Git可管理的仓库

   ```
   $ git init
   Initialized empty Git repository in /Users/User/hello/.git/
   ```

3. 打开VS Code,在源代码管理中找到hello文件夹，在文件夹中新建文档`hello.md`，并在文档中键入`Hello,余晨晓!`

4. 点击源代码管理，将所有更改存入暂存区，然后在消息栏填写`init: add hello files`点击提交。(在VS Code的源代码管理界面提交相当于执行了`git add .`和`git commit -m`)

5. 关联远程仓库。在终端输入`git remote add origin <远程仓库地址>`将远程仓库地址添加为origin

6. 用`git push -u origin main`推送本地提交到远程。

   > https://github.com/shakarobia35/Tasks/tree/main/githello-main

### 遇到的问题和解决办法
> 传输时遇到的问题和补充知识

1. 我想复制A仓库的内容并把它移动到B仓库该怎么做?

   点击A仓库Code键，选择SSH下载其文件夹(如果是压缩包最好解压缩)，再点击B仓库Add files上传文件夹。

2.对文件进行修改
   + `git branch`查看本地分支名(查看是main还是master)；
   + `cat readme.txt`显示文件内容；
   + **回溯过去的代码信息**

    > + `git diff`查看修改内容;
    > + `git log`显示从最近到最远的提交日志,`git log --name-status -1`查看最近一次提交中包含的文件；
    > + `git status`检查本地与远程的同步状态；

   + **把文件退回到上个版本**

    >  + `git reset --hard HEAD^`将版本退回到上个版本的已提交状态，`--soft`是退回到未提交状态，`--mixed`会回退到上个版本已添加但未提交的状态;`git reset --hard 1094a`指定回到1094a这个版本；

   + **恢复未来版本**

    > + `git reflog`记录每一次命令，可以据此查找commit id，再用reset恢复;
    > + `git push origin master:main`将本地的master分支推送到远程的main分支；
    > + `git pull origin master --allow-unrelated-histories`拉取远程仓库的内容并合并；
    >
     ```
     git add readme.txt /把文件添加到暂存区
     git commit -m "说明" /把暂存区所有内容提交到Git自建分支master
     git push origin main
     ```

## Git原理

### 工作区和暂存区

**工作区**:在电脑里能看到的目录。

**版本库**:工作区的隐藏目录`.git`。含有称为stage（或index）的**暂存区**，Git自动创建的第一个分支`master`，以及指向`master`的一个指针`HEAD`。
[工作区、版本库分布图片](https://liaoxuefeng.com/books/git/time-travel/working-stage/repo.png)

### 基于文档
#### 修改
git管理的是修改，如果某次修改后没有使用`git add`命令，那么这次修改就不会放入暂存区，也就不会在输入`git commit`时被提交。要想让这次修改有效，必须先存入暂存区。即:

> 第一次修改 -> `git add` -> 第二次修改 ->` git add` -> git commit

提交代码后，用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别。

#### 撤销修改

+ 作修改了，但还没添加到暂存区:

`git checkout -- file`可以丢弃工作区的修改，把文件恢复到上一个版本的状态。[git checkout不同用法](#switch)

命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销。这里有两种情况：

> 一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
>
> 一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

+ 作修改后添加到暂存区了，但还没有提交:

  用命令`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区。此时的情况就和第一点一样了，依据第一点的步骤可以实现目的。

  `git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。

+ 已经提交了不合适的修改到版本库，想要撤销本次提交:进行**版本回退**(前提:代码暂未推送到远程库)

#### 删除
`rm text.txt` 用`rm`命令把文件删除(或直接手动在文件管理器中把没用的文件删除)
此时工作区和版本库储存内容就不一致了
> 注意，`rm`是操作系统提供的命令，它只会在文件系统层面(从电脑硬盘上)删除文件.但是这个操作不会通知Git，也不会更新Git的索引（暂存区） 。Git此时只会检测到该文件在***工作区***消失了，但记录仍然存在于***版本库***中。

因此，如果希望通过Git记录并同步这次删除操作，在执行`rm`命令后，还需要额外执行`git add test.txt`或`git rm test.txt`来更新暂存区，再`git commit`进行提交。
*如果删错了呢?*
用`$ git checkout -- test.txt`可以把误删的文件恢复到最新版本。(**前提**:这个被误删的文件必须是已经被添加到版本库的文档)
## 进阶功能
### 远程仓库
*只要不在同一个目录下,一台电脑上也可以克隆多个版本库.但是在一台电脑上克隆多个远程库完全没有意义。*
### 先有本地库，后有远程库时，如何关联远程库?
#### 获得Git远程仓库
*本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，故首先需要创建SSH Key*
```
 Windows下打开Git Bash,用`$ ssh-keygen -t rsa -C "770733919@qq.com"`创建SSH Key,然后一路回车，使用默认值,无需设置密码。
 用户主目录里会出现`ssh`目录，里面有`id_rsa`私钥和`id_rsa.pub`共钥两个SSH Key的秘钥对。
 登陆GitHub，打开“Account settings”，“SSH Keys”页面，点“Add SSH Key”，填写Title，在Key文本框里粘贴id_rsa.pub文件的内容，点“Add Key”SSH Key就创建成功了。
```
#### 把本地库内容推送到远程库
新建仓库learngit，打开git bash,运行`$ git remote add origin git@github.com:shakarobia35/learngit.git`添加后，远程库的名字就是origin。
再就可以用`$ git push -u origin master`把本地库的所有内容推送到远程库上。
> `git push`实际上是把当前分支master推送到远程。
> 由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

**此后，只要本地作了提交(指的是修改或新增文件，将修改提交到暂存区，提交到本地仓库这几个步骤)，就可用`$ git push origin master`把本地master分支的最新修改推送至GitHub。**
> ps:创建新文件用`echo "# 描述理由" > README.md`

***SSH警告***
第一次使用Git的clone或者push命令连接GitHub时，会得到警告： 
```
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```
这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可。Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了.
#### 删除远程库
先用git remote -v查看远程库信息，然后，用`$ git remote rm origin`根据名字删除远程库(此处删掉的是origin)
> 此处的“删除”只是解除了本地和远程的绑定关系，而远程库本身并没有被改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。
### 若从零开发，该怎么实现文件互传?
#### 创建远程库
登陆GitHub，创建新仓库`gitkills`,勾选`Initialize this repository with a README`，自动创建README.md文件。
在git bash上用`git clone git@github.com:shakarobia35/gitskills.git`克隆一个本地库.此时再查看仓库目录(cd gitskills)，就已经存在README.md文件了.
> 要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
> Git支持多种协议，包括https，但ssh协议速度最快。

### [分支管理](https://liaoxuefeng.com/books/git/branch/create/index.html)
Git把每次提交的记录串成一条时间线，形成分支。当前只有一条分支---`master`分支。
一开始,master分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`。这种功能可以确定当前分支及当前分支的提交点。

**创建新的分支**
如dev时，Git就会新建dev指针，指向和master相同的最新提交，再把HEAD指向dev，表示当前分支在dev上。(注意:此时HEAD不再指向master，再做修改是在dev的分支上进行。再修改后，***master会滞后于dev***)

> `$ git checkout -b dev`创建dev分支并切换到dev分支(`git checkout`+`-b`是创建并切换=`$ git branch dev`和`$ git checkout dev`)
> 
> 用git branch可以查看当前分支(带有*)，显示为`* dev
  master`
>
> 此后就可以在dev分支上正常修改提交。比如对readme.txt进行修改后提交(`$ git add readme.txt 
`和`$ git commit -m "branch test"`）

**合并分支到主分支**
把master指向dev当前提交的最新版本处,切换回master分支(就是改指针)即`$ git checkout master`
输入之后，HEAD重新指向master,master仍滞后于dev。
修改过后的文件 readme.txt添加的内容提交在dev里，故当我们查看文件的时候(现在我们处在master分支)，看不到修改的内容(master分支此刻的提交点没有改变)。
用`$ git merge dev`把dev分支的工作成果合并到master分支上。

> `git merge`命令用于合并指定分支到当前分支.
> 输入后会出现Fast-forward信息，这是在告诉我们这次合并是“快进模式”——把master直接指向dev的当前提交
合并完成后就可以用`$ git branch -d dev`删除dev分支。删除后，用`git branch`查看，可以发现只剩下master分支(* master)

*因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。*
### switch

切换分支使用`git checkout <branch>`，而撤销修改是`git checkout -- <file>`

而更科学的切换分支做法是使用`switch`命令:

> 创建并切换到新的`dev`分支`$ git switch -c dev`
>
> 直接切换到已有的`master`分支`$ git switch master`

***

#### 回忆

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`或者`git switch <name>`

创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

------





