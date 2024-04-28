## Git

### Git 基本概念

#### Git 历史

Git 是最流行的分布式版本控制系统（Distributed Version Control System，简称 DVCS）。它由 Linus Torvalds 创建，当时非常需要一个快速、高效和大规模分布式的源代码管理系统，用于管理 Linux 源代码。

由于 Linus 对几乎所有现有的源代码管理系统抱有强烈的反感，因此他决定编写自己的源代码管理系列。2005 年 4 月，Git 就诞生了。到了 2005 年 7 月，维护工作就交给了 Junio Hamano，自此他就一直在维护这个项目。

虽然最初只用于 Linux 内核，但 Git 项目迅速传播，并很快被用于管理许多其他 Linux 项目。现在，几乎所有的软件开发，尤其是在开源世界中，都是通过 Git 进行的。

#### 版本控制系统

版本控制是指对软件开发过程中各种程序代码、配置文件及说明文档等文件变更的管理，是软件配置管理的核心思想之一。版本控制技术是团队协作开发的桥梁，助力于多人协作同步进行大型项目开发。软件版本控制系统的核心任务就是查阅项目历史操作记录、实现协同开发。

常见版本控制主要有两种：**集中式版本控制**和**分布式版本控制**。

##### 集中式版本控制系统

集中式版本控制系统，版本库是集中存放在中央服务器的。工作时，每个人都要先从中央服务器获取最新的版本。完成之后，再把自己添加/修改的内容提交到中央服务器。所有文件和历史数据都存储在中央服务器上。SVN 是最流行的集中式版本控制系统之一。

集中式版本控制系统的缺点就是必须联网才能使用，如果使用局域网还好，速度会比较快。而如果是使用互联网，网速慢的话，就可能需要等待很长时间。除此之外，如果中央服务器出现故障，那么版本控制将不可用。如果中心数据库损坏，若数据未备份，数据就会丢失。

![1712412386333](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712412386333.png)

##### 分布式版本控制系统

分布式版本控制系统，每台终端都可以保存版本库，版本库可以不同，可以对每个版本库进行修改，修改完成后可以集中进行更新。虽然它没有中心服务器，但可以有一个备份服务器，它的功能有点类似于 SVN 的中央服务器，但它的作用仅是方便交换修改，而不像 SVN 那样还要负责源代码的管理。Git 是最流行的分布式版本控制系统之一。

和集中式版本控制系统相比，分布式版本控制系统的安全性要高很多，因为每个人电脑里都有完整的版本库，某一个人的电脑损坏不会影响到协作的其他人。

![1712412420468](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712412420468.png)

#### SVN vs Git

Git 相较于 SVN：

- **提交速度更快：** 因为在 SVN 中需要更频繁地提交到中央存储库，所以网络流量会减慢每个人的速度。而使用 Git，主要在本地存储库上工作，只需每隔一段时间才提交到中央存储库。
- **没有单点故障：** 使用 SVN，如果中央存储库出现故障，则在修复存储库之前，其他开发人员无法提交他们的代码。使用 Git，每个开发人员都有自己的存储库，因此中央存储库是否损坏并不重要。开发人员可以继续在本地提交代码，直到中央存储库被修复，然后就可以推送他们的更改；
- **可以离线使用：** 与 SVN 不同，Git 可以离线工作，即使网络失去连接，也可以继续工作而不会丢失功能。

![1712412486047](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712412486047.png)

#### Git 安装

在Git官网下载、安装即可：https://git-scm.com/download

![1712412521592](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712412521592.png)

安装完成之后，可以使用以下命令来查看 Git 是否安装成功：

```
git --version
```

如果安装成功，终端会打印安装的 Git 的版本：

![1712412559739](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712412559739.png)

#### Git 结构和状态

从Git的角度来看，可以在三个区域进行文件更改：工作区，暂存区和存储库。

- **工作区：** 本地看到的工作目录；
- **暂存区：** 一般存放在 `.git` 目录下的 index 文件（.git/index）中，所以暂存区有时也叫作索引（index）。暂存区是一个临时保存修改文件的地方；
- **版本库：** 工作区有一个隐藏目录 `.git`，这个不算工作区，而是 Git 的版本库，版本库中存储了很多配置信息、日志信息和文件版本信息等。

![1712412808074](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712412808074.png)

Git 工作目录下的文件存在两种状态：

- untracked：未跟踪，未被纳入版本控制，即该文件没有被Git版本管理；
- tracked：已跟踪，被纳入版本控制，即该文件已被Git版本管理。

其中**已跟踪状态**可以细分为以下三种状态：

- Unmodified：未修改状态
- Modified：已修改状态
- Staged：已暂存状态

可以通过运行以下命令来检查当前分支的状态：

```
git status
```

显示结果如下：

![1712412838372](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712412838372.png)

此命令不会更改或更新任何内容。它会打印出哪些文件被修改、暂存或未跟踪。未跟踪的文件是尚未添加到 git 索引的文件，而自上次提交以来已更改的文件将被视为已被 git 修改。

### 全局配置

当安装Git后首先要做的就是配置所有本地存储库中使用的用户信息。每次Git提交都会使用该用户信息。

config 命令适用于不同的级别：

- **本地级别：** 所有配置都仅限于项目目录。默认情况下， 如果未通过任何配置, 则git config将在本地级别写入；
- **全局级别：** 此配置特定于操作系统上的用户，配置值位于用户的主目录中；
- **系统级别：** 这些配置放在系统的根路径下，它跟踪操作系统上的所有用户和所有存储库。

下面的配置均为写入系统级别：

#### 设置用户名

可以使用以下命令设置使用 Git 时的用户名：

```js
git config --global user.name "name"
// git config user.name "name"
```

可以使用以下命令查看设置的`user.name`：

```js
git config user.name
```

#### 设置邮箱

可以使用以下命令设置使用 Git 时的邮箱：

```js
git config --global user.email "email"
// git config user.email "email"
```

可以使用以下命令查看设置的 email：

```js
git config user.email
```

#### 设置命令颜色

除了上述两个基本的设置之外，还可以设置命令的颜色，以使输出具有更高的可读性：

```js
 git config --global color.ui auto
```

#### 查看所有配置

通过上面的命令设置的信息会保存在本地的`.gitconfig`文件中。可以使用以下命令查看所有配置信息：

```
git config --list
```

如果在全局输入这个命令，就会显示全局的配置：

![1712413041430](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712413041430.png)

如果在使用 Git 的项目中输入该命令，除了会显示全局的配置之外，还会显式本地仓库的一些配置信息，如下：

![1712413075815](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712413075815.png)

#### 设置别名

git config 命令为我们提供了一种创建别名的方法，这种别名通常用于缩短现有的命令或者创建自定义命令。来看一个例子：

```js
git config --global alias.cm "commit -m"
```

这里为`commit -m`创建一个别名 `cm`，这样在提交暂存区文件时，只需要输入以下命令即可：

```js
git cm <message>
```

#### 注意

> 修改文件也可以达到上面使用命令行的效果

```
在 .git --> config 中可以查看和修改  name email (修改单个的git项目)

在 C盘 --> 用户 --> Administrator .gitconfig 中可以查看和修改  name email (全局)
```

![1713254365304](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713254365304.png)

### 常见问题

#### Git对文件名大小写不敏感

> 例如我在这样一个项目中![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713688874434.png)某一天，不知道啥情况，需要我把文件夹名称改一下。
>
> ![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713688889433.png)这个时候，提交列表是没有东西让你提交的，也没有提示报错（当然可能有的伙伴用了某些vscode插件会提示），所以你可能会接着写代码,然后提交代码
>
> ![1713688909399](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713688909399.png)这个时候，我们去git上面去看
>
> ![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713688933000.png)发现大小写改了没有效果？
>
> ## 分析
>
> 刚开始时候，本地和远程都是大写的。![1713688958483](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713688958483.png)后面，你把本地改为小写的。![1713688967321](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713688967321.png)但是！git它是不知道的，因为git在默认情况下，是忽略掉文件名大小写的，它不会认为有任何更改。
>
> 然后接下来你把本地的推送到远程，由于git并不清楚这些文件有任何的更改，所以，远程那边它仍然保持是大写，因为文件都没有动，只是动了我们新建的index.js文件，文件内引用的是小写，远程是大写
>
> ![1713688983865](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713688983865.png)
>
> 这就会造成一个情况，你本地是好好的，到了远程就报错了，别人拉新代码去运行，却发现报错。都是一套代码！你可能烧到cpu了，搞不清楚为什么报错。后面才发现是路径大小写不一样，这就浪费太多时间啦。
>
> ## 解决
>
> 所以最好的做法是什么？那就是你一开始就告诉git。你不要忽略大小写，而是要保持大小写敏感，那具体怎么做呢，那做法就非常简单了，只需要输入一个命令
>
> ```js
> git config core.ignorecase false
> ```
>
> 表示忽略大小写这个配置给它禁用，禁用了过后你再来看看。![1713689029028](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689029028.png)禁用过后，它就能跟踪到了，这个时候提交一下。再看看远程。![1713689043605](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689043605.png)这个时候发现，小写的文件夹是到远程了，但是之前的那个文件夹保留下来了，那怎么解决呢？
>
> 使用下面命令
>
> ```
> git rm --cached Components -r 
> ```
>
> 命令用于把暂存区的指定的文件移除![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689083019.png)提交之后，我们再到远程看看。![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689092095.png)

```js
# 忽略大小写
git config core-ignorecase true 
# 区分大小写
git config core-ignorecase false 
```

### Git版本号

```js
# 每次提交都会生成一个版本号，这个版本号是40位长度的16进制的数字字符串。经过特殊的加密算法计算出来的，不容易重复。
1. 避免版本合并的时候冲突
2. 定位仓库中的文件，把40位拆分成两部分：前两位是文件夹，后38位是文件名（.git-->objects）
// 当依据本次提交的commit版本号在.git的objects文件夹中找到文件，打开后是一堆乱码。如何查看里边的内容：
// 回到.git文件夹的目录中，右键选择Git Bash Here打开git自带的命令行工具，使用命令查看这个文件。
// git cat-file -p 版本号
// -p 友好的查看我的文件
// 得到的是本次提交的内容
```

![1713235627811](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713235627811.png)

```js
# git如何记录那个是最新的版本号
.git --> HEAD 中记录一个路径：ref: refs/heads/main
.git --> refs --> heads --> main 在main中记录着一个版本号

// 而main则是 分支的名称
# 当分支发生变化后 HEAD中也会发生改变
// 切换 dev 分支 HEAD中 ref: refs/heads/dev
```



### Git命令

![1709187444673](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709187444673.png)

```
git add 文件名  追踪这个文件
git status 查看状态 红色没追踪 绿色追踪
git add . 所有的文件
git rm --cached 文件名（可多个） 取消追踪
git checkout -- 文件名 还原成修改前的状态
```

##### Git init

```js
git init // 命令用于初始化项目文件夹中的新存储库。
// 本地初始化(初始化git环境), 这个会生成一个.git目录
```

初始化之后，就会创建一个名为`.git`的新子文件夹，其中包含 Git 将用于跟踪项目更改的多个文件和更多子目录：

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712412691306.png)

在使用 Git 进行代码管理时，不希望一些文件出现在跟踪列表中，比如`node_modules`文件。这种情况下，可以在项目的根目录中创建一个名为.gitignore的文件，在该文件中列出要忽略的文件和文件夹，来看一个示例：

```js
# 所有以.md结尾的文件
*.md  

# lib.a不能被忽略
!lib.a

# node_modules和.vscode文件被忽略
node_modules
.vscode

# build目录下的文件被忽略
build/

# doc目录下的.txt文件被忽略
doc/*.txt

# doc目录下多层目录的所有以.pdf结尾的文件被忽略
doc/**/*.pdf
```

注意：以 # 符号开头的行是注释。

##### Git status

```js
git status // 命令用于检查存储库的状态（例如未跟踪的文件和准备提交的更改） 暂存区的状态
```

##### Git查看提交

###### Git show

```js
// 查看具体提交，具体修改等待
Git show
// or
Git log -n1 -p
```

![1713602804521](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713602804521.png)

###### Git log

```js
git log // 命令用于显示所有提交的历史记录。它包括提交哈希、作者、日期和提交消息。
git log --oneline // 将每次commit提交,简化成一行
```

##### Git diff

```js
git diff // 命令用于检查当前工作目录和上次提交之间的差异。
```

##### Git pull

```js
git pull // 命令用于从远程存储库获取更改并将其集成到本地分支中。

git pull origin 分支 // 更新本地仓库的某一个分支的代码 (相当于 pull = fetch + merge)
# 它本质上是两个独立的 Git 命令的组合：git fetch 和 git merge。
```

##### Git fetch

```js
git fetch origin 分支 // 同步远程仓库的分支信息
```

##### Git merge

```js
git merge // 命令将所有分支更改合并到一个分支中。
git merge <branch_name> // 将 <branch_name> 替换为要将其更改合并到当前分支的分支名称。
```

##### Git restore

```js
git restore filename // 将删除的文件恢复
// 这个删除的文件是已标记过的
// 当删除文件后已提交
git reset --hard commitid // commitid之后的提交会消失
// 想要保留所有提交，切还原
git revert commitid
```

#### 分支

##### Git 创建分支

```js
git branch <branchname>
git checkout -b <local branchname> // 创建本地分支,并切换到本地分支
git checkout -b <local branchname> <origin/branchname> // 创建本地分支,并切换到本地分支,并且这这种方法创建的本地分支会和远程分支建立映射关系

git push -u origin <branch> // 将新建的本地分支推送到远程仓库，以在远程仓库添加这个分支
```

##### Git 切换分支

```js
git checkout <local branchname> // 切换到本地分支
git checkout -b <local branchname> // 创建本地分支,并切换到新创建的分支上
git checkout -b <local branchname> <origin/branchname> // 创建本地分支,并切换到本地分支,并且这这种方法创建的本地分支会和远程分支建立映射关系

git switch 分支; git checkout 分支; // 切换分支

git switch -c 分支; git checkout -b 分支 // 创建分支
```

##### Git 重命名分支

```js
# 将分支重命名：

git branch -m <oldname> <newname>

# 如果newname名字分支已经存在，则需要使用-M来强制重命名：

git branch -M <oldname> <newname>
```

##### Git 合并分支

实际开发工作的时候，我们都是在自己的分支开发，然后将自己的分合并到主分支，那合并分支用2种操作，这2种操作有什么区别呢？

git上新建一个项目，默认是有master分支的，将项目克隆到本地，我们的准备工作就完成了

![1712998862505](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712998862505.png)

> ### 同学A：
>
> 执行git log ，可以看到有一个提交记录，是初始化提交
>
> ![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712998896792.png)新增一个文件a.txt, 再次查看我们的提交记录，有2条提交记录了
>
> ![1712998907388](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712998907388.png)
>
> 这个时候将本地新commit的记录push到远程仓库，就可以看到我们的2次提交了
>
> ![1712998922560](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712998922560.png)

> ### 同学B：
>
> 同学B在已经有提交记录的master分支上，检出分支dev，并将分支推送到远程分支，并进行自己的开发
>
> ![1712998967582](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712998967582.png)
>
> 查看远程仓库，多了一个dev分支
>
> ![1712998981529](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712998981529.png)
>
> 此时的git分支类图是这样的
>
> ![1712998997589](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712998997589.png)
>
> 此时B同学开始进行开发，完成了自己的3次提交工作，使用git log 看一下
>
> ![1712999010074](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712999010074.png)此时git的分支类图是这样子的
>
> ![1712999021361](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712999021361.png)

现在有这样一个现实的请况，就是B同学准备进行第4次提交的时候，同学A在master主分支上进行了一次提交，master的提交已经向前走了

此时的git分支类图是这样的

![1712999054746](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712999054746.png)

此时我们知道B同学开发的dev分支是基于C2提交点切出来的，而这个时候master分支已经被更新了

如果B同学开发完毕，需要将其所作的功能合并到master分支 ，他可以有两种选择：

###### git merge

- （1）找到master和dev的共同祖先，即C2
- （2）将dev的最新提交C5和master的最新提交即C6合并成一个新的提交C7，有冲突的话，解决冲突
- （3）将C2之后的dev和master所有提交点，按照提交时间合并到master

![1712999110757](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712999110757.png)

```js
# 如果想将A分支合并到B分支，就要先切换到B分支，然后执行命令：git merge A。
git merge <branch>
```

```
git add . 暂存缓存区
Git commit -m ‘’ 提交本地仓库
Git branch -a 查看所有分支
Git checkout 远程分支
Git status
Git pull
Git log
Git checkout 本地分支a
Git merge 本地分支b （将本地分支b合并到本地分支a）
有冲突解决冲突（在分支上有一个表示 MING 有冲突）
Git status
Git commit -m ‘’
Git push  （推送到远程仓库后，在远程仓库中提交自己的分支合并到需要的分支上）
```

###### git rebase

切换分支到需要rebase的分支，这里是dev分支

执行git rebase master，有冲突就解决冲突，解决后直接git add . 再git rebase --continue即可

发现采用rebase的方式进行分支合并，整个master分支并没有多出一个新的commit，原来dev分支上的那几次（C3，C4，C5）commit记录在rebase之后其hash值发生了变化，不在是当初在dev分支上提交的时候的hash值了，但是提交的内容被全部复制保留了，并且整个master分支的commit记录呈线性记录

此时git的分支类图

![1712999157887](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712999157887.png)

git merge 会让2个分支的提交按照提交时间进行排序，并且会把最新的2个commit合并成一个commit。最后的分支树呈现非线性的结构

git reabse 将dev的当前提交复制到master的最新提交之后，会形成一个线性的分支树

##### Git 删除分支

```js
Git branch -d branchname // 删除本地分支,需要注意，在删除分支之前，要先切换到其他分支，否则就会报错

# 有时 Git 会拒绝删除本地分支，因为要删的分支可能有未提交的代码。这是为了保护代码以避免意外丢失数据。可以使用以下命令来强制删除本地分支：
git branch -D <branch>

# 删除远程分支
git push origin --delete <name>
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712413533969.png" alt="1712413533969" style="zoom:50%;" />

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712413571317.png" alt="1712413571317" style="zoom:50%;" />

##### Git 查看分支

```js
当通过命令：
$ git branch -a  // 查看所有分支:本地&远程
$ git branch -r  // 查看所有分支:远程
$ git branch // 罗列分支,该命令可以列出项目所有的本地分支，显示绿色的分支就是当前分支

查看不到远程dev分支时，可以通过命令：
$ git fetch  // 进行刷新，然后再通过 
$ git branch -a   // 就可以看到远程分支了。
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712413232226.png" alt="1712413232226" style="zoom:33%;" />

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712413250570.png" alt="1712413250570" style="zoom:33%;" />

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712413306629.png" alt="1712413306629" style="zoom:33%;" />

##### Git 查看当前分支是基于哪个分支创建的

```
切换到要查询的分支下
git checkout branchname
使用命令：
git reflog --date=local --all | grep 后面加上要查询的分支名称
```

##### Git 将本地与远程分支关联

**Git 将本地分支与远程分支关联（不同名也可以），并提交**

```
1. 查看本地分支与远程分支是否关联 git branch -vv;有蓝色分支是关联，没有就是没有关联的远程分支
```

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps53.jpg) 

```js
2. feature/performance-md 只是一个本地分支；

// 想要关联就 
git branch -u origin/feature/performance
#或者
git branch --set-upstream-to=origin/feature/performance
// 撤销本地分支与远程分支的映射关系
git branch --unset-upstream
```

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps54.jpg)

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps55.jpg) 

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps56.jpg) 

```
3. 这样就可以将本地分支的feature/performance-md推送到 远程分支origin/feature/performance，也可以拉去远程分支代码

4. 推送到远程，注意方式不一样，要多一个 " head: "  git push origin HEAD:feature/performance

Git push 时会有提示
```

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps57.jpg) 

 ![1709187760764](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709187760764.png)

##### Git 分支规范

```
feat: 新功能feature的缩写
perf: 改进performance的缩写
fix: 修复bug
refactor: 重构代码(既不是新增功能，也不是修复bug的改动)
chore: 小改动 (既不是新增功能，也不是修复bug的改动)
test: 测试代码

master 主分支 有且仅有一个主分支。所有提供给用户的正式版本，都在主分支上进行发布
develop 开发分支 日常开发应在开发分支中完成
feature/* 功能分支 为了开发特定功能。从develop分支中开发，开发完成后，将代码合并到develop分支中。以feature/*或feat/*命名
release/* 预发布/UAT分支 从develop分支中分出来进行UAT，UAT测试过程中的BUG修复在该分支上完成，预发布完成后，将代码合并到develop分支和master分支中，以release/*命名
fix/* 修复分支 软件正式发布以后，修复bug的分支。从master分支上分出来，修复完成后，将代码合并到develop分支和master分支中，以fix/*命名


git:分支开发
git 是分布式的版本管理系统，svn 是集中式版本管理系统。
master 分支--主分支
master 为主分支，也是用于部署生产环境的分支，确保master分支稳定性， 
master 分支一般由develop以及hotfix分支合并，任何时间都不能直接修改代码。

develop 分支--开发分支
develop 为开发分支，始终保持最新完成以及bug修复后的代码，
一般开发的新功能时，feature分支都是基于develop分支下创建的。

feature 分支--开发新功能
开发新功能时，以develop为基础创建feature分支。
分支命名: feature/ 开头的为特性分支， 命名规则: feature/user_module、 feature/cart_module

release分支--预上线分支
release 为预上线分支，发布提测阶段，会release分支代码为基准提测。
当有一组feature开发完成，首先会合并到develop分支，进入提测时会创建release分支。

如果测试过程中若存在bug需要修复，则直接由开发者在release分支修复并提交。
当测试完成之后，合并release分支到master和develop分支，此时master为最新代码，用作上线。

hotfix 分支--修复分支
分支命名: hotfix/ 开头的为修复分支，它的命名规则与feature分支类似。
线上出现紧急问题时，需要及时修复，以master分支为基线，创建hotfix分支，
修复完成后，需要合并到master分支和develop分支
```

##### Git 克隆

```js
Git clone url(http地址)
Git clone url filename // 克隆项目并修改文件夹名称
Git clone -b branchname remoteaddr // 克隆指定分支 remoteaddr （http地址）
```

#### 版本回退

##### HEAD指针

```
HEAD : HEAD指向当前分支的最后一次commit
HEAD^ : 当前版本的前一个版本
HEAD^^ : 当前版本的前两个版本
HEAD~100 : 当前版本的前100个版本
```

##### Git **版本回退**

```
git reset HEAD^ : 回退到当前版本的前一个版本
git reset HEAD^^ : 回退到当前版本的前两个版本
git reset HEAD~100 : 回退到当前版本的前100个版本
如果增加 --hard 参数会重置工作区和暂存区,未提交的变更将不可逆的丢弃
```

```
1. Git log 查看commit 提交；复制 版本号

2. 使用 git reset --hard 版本号 

3. Git push -f origin 分支
```

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps58.jpg)

// TODO git stash 补存
#### 提交

##### Git 提交暂存区

```js
git add <file_name> // 将 <file_name> 替换为你的文件名
git add *.txt // 将所有的.txt文件提交到暂存区
git add . // 提交所有的更改到暂存区
```

##### Git 取消暂存

```
git reset <file_name>
git rm --cached <file>
```

##### Git stash 缓存

git stash 命令用于临时保存尚未准备好提交的更改。

```js
## stash 是一种暂存工作区更改的方法,它允许你暂存保存未提交的更改,并将当前的工作目录恢复到上次提交的状态.这对于需要切换分支处理其他问题、或者当前的工作尚未完成但需要清理工作区以拉取或合并其他分支时非常有用.
git stash // 此命令会将当前工作目录中的所有未提交更改(包括已添加至暂存区的)保存到一个新的stash项目中,并重置工作目录为上一次提交的状态.
git stash clear // 删除所有的stash
git stash save "message" // 可以自定义stash的描述信息,并执行stash操作
git stash branch <branchname> [<stash@{n}>] // 基于某个stash创建新的分支,并自动应用该stash中更改
git stash list // 显示所有已缓存的stash列表及其简短描述
git stash apply [stash@{n}] // 从stash列表中取出并应用指定编号(n)的stash,如果没有指定,则默认使用最近的一个stash.注意,这并不会删除stash,它仍然存在于列表中.
git stash pop // 与apply类似,但它会在应用stash后立即将其从stash列表中移除.
git stash pop [<stash@{n}>] // 删除指定的stash,不指定则删除最近的stash
git stash apply --index 或者 git stash pop --index // 如果stash包含索引修改(即暂存区的更改),这个选项会使Git尝试将这些更改精确地应用回暂存区,而不仅仅是工作目录
git stash show [-p | --patch] [<stash@{n}>] //显示指定stash包含的更改详情,如果不指定stash编号,则默认显示最近的stash.加上-p或者--patch参数可以看到差异补丁格式的详细内容
```

##### Git提交本地仓库

```js
git commit -m '注释' // 将暂存区的文件修改提交到本地仓库

# 如果上次提交暂存的messge写错了怎么办呢？可以使用使用以下命令来更新提交，而不需要撤销并重新提交：

git commit --amend -m <message>

#如果有一个新的文件修改，也想提交到上一个commit中，可以使用以下命令来保持相同的提交信息：

git add .
git commit --amend --no-edit
```

##### Git提交信息(commit message)写错了

```js
# 如果你的提交信息(commit message)写错了且这次提交(commit)还没有推(push), 你可以通过下面的方法来修改提交信息(commit message):

$ git commit --amend --only

# 这会打开你的默认编辑器, 在这里你可以编辑信息. 另一方面, 你也可以用一条命令一次完成:

$ git commit --amend --only -m 'xxxxxxx'

# 如果你已经推(push)了这次提交(commit), 你可以修改这次提交(commit)然后强推(force push), 但是不推荐这么做。
```

##### Git修改提交的commit

```js
# 提交到本地仓库
git commit --amend // 弹出一个操作框(需要会点基本的vim知识: 我们输入i 进行对commit的修改,当我们改完后输入 esc 表示改完了,在输入 :wq 就可以保存了)
```

##### Git合并多个commit

```js
# 提交到本地仓库
git commit --amend --no-edit // 将修改的代码合并到上一个commit中

# 或
git rebase -i [commit Id] //commit Id 是你要合并的几个commit中最老的commit Id

```

##### Git提交远程仓库

```js
git push //用于将提交发送到远程存储库。这只会推送已提交的更改
git push origin <branch_name>
git push -u origin <branch_name> // 如果分支是新创建的，那么需要使用以下命令上传分支
```

##### Git 合并指定提交

在不同分支之间进行代码合并时，通常会有两种情况：一种情况是需要另一个分支的所有代码变动，那么就可以直接合并（git merge），另一种情况是只需要部分代码的变动（某几次提交），这时就可以使用以下命令来合并指定的提交：

```
git cherry-pick <commit hash>
```

建议添加 `-x` 标志，因为它会生成标准化的提交消息，通知用户它是从哪里pick出来的：

```
git cherry-pick -x <commit hash>
```

那么这个`commit hash`是从哪里来的呢？可以在需要被合并的分支上执行以下命令：

```
git log
```

这时终端就会显示出所有的提交信息：

![1712414459219](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712414459219.png)

这里黄色文字中`commit`后面的部分就是`commit hash`，复制即可。

##### Git 检查提交

Git允许我们在本地检查特定的提交。输入以下命令就可以查看有关当前提交的详细信息：

```
git show
```

输出的结构如下，可以看到，它显示出了上次提交的commit id、作者信息（邮箱和姓名）、提交日期、commit message、代码diff等：

![1712414700627](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712414700627.png)

还可以使用`HEAD~n`语法或提交哈希来检查过去的提交。使用以下命令就可以获取往前数的第三次提交的详细信息：

```
git show HEAD~3
```

除此之外，还可以添加一个`--oneline`标志，以简化输出信息：

```
git show --oneline
```

这样提交信息就简洁了很多：

![1712414728186](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712414728186.png)

##### Git 查看贡献者

可以使用以下命令来返回每个贡献者的commit次数以及每次commit的commit message：

```
$ git shortlog
```

其可以添加两个参数：

- s：省略每次 commit 的注释，仅仅返回一个简单的统计。
- n：按照 commit 数量从多到少的顺利对用户进行排序。

加上这两个参数之后就可以看到每个用户中的提交次数以及排名情况：

```
git shortlog -sn
```

这样就会显示出该项目所有贡献者的commit次数，从上到下依次减小：

![1712414768286](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712414768286.png)

除此之外，还可以添加一个`--no-merges`标志，以忽略合并提交的次数：

```
 git shortlog -sn --no-merges
```

#### 远程操作

##### Git 查看远程仓库

可以使用以下命令来查看远程仓库：

```
git remote
```

该命令会列出指定的每一个远程服务器的简写。如果已经克隆了远程仓库，那么至少应该能看到 origin ，这是 Git 克隆的仓库服务器的默认名字：

![1712414854720](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712414854720.png)

可以执行以下命令来获取远程仓库的地址：

```
git remote -v
```

其中`fetch`是获取，`push`是推送：

![1712414878117](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712414878117.png)

可以使用以下命令来查看更加详细的信息：

```
git remote show origin
```

输出结果如下：

![1712415321347](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712415321347.png)

##### Git 添加远程仓库

可以使用以下命令来将本地项目链接到远程仓库：

```
git remote add <remote_name> <remote_url>
```

其中：

- `remote_name`：仓库名称（默认是origin）
- `remote_url`：远程仓库地址

该命令允许 Git 跟踪远程存储库并将本地存储库连接到远程仓库。

##### Git 移除远程仓库

可以使用命令来移除远程仓库：

```
git remote rm origin
```

需要注意，该命令只是从本地移除远程仓库的记录（也就是解除本地仓库和远程仓库的关系），并不会真正影响到远程仓库。

##### Git 从远程仓库抓取与拉取

可以使用以下命令来从远程仓库获取最新版本到本地仓库，不会自动merge（合并数据）：

```
git fetch 
```

由于该命令不会自定合并数据，所以该命令执行完后需要手动执行 git merge 远程分支到所在的分支。

可以使用以下命令来将**远程指定分支**拉取到**本地指定分支上**：

```
git pull origin <远程分支名>:<本地分支名>
```

使用以下命令来将**远程指定分支**拉取到**本地当前分支上**：

```
git pull origin <远程分支名>
```

使用以下命令开将**与本地当前分支同名的远程分支**拉取到**本地当前分支上：**

```
git pull
```

注意：如果当前本地仓库不是从远程仓库克隆，而是本地创建的仓库，并且仓库中存在文件，此时再从远程仓库拉取文件的时候会报错（`fatal: refusing to merge unrelated histories` ），解决此问题可以在`git pull`命令后加入参数`--allow-unrelated-histories`，即：

```
git pull --allow-unrelated-histories
```

##### Git 推送到远程仓库

可以使用以下命令将**本地指定分支**推送到**远程指定分支上**：

```
git push origin <本地分支名>:<远程分支名>
```

可以使用以下命令将**本地指定分支**推送到**与本地当前分支同名的远程分支上：**

```
git push origin <本地分支名>
```

使用以下命令将**本地当前分支**推送到**与本地当前分支同名的远程分支上**：

```
git push
```

可以使用以下命令来将本地分支与远程同名分支相关联：

```
git push -u origin <本地分支名>
```

由于远程库是空的，第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令为git push。

#### 修改操作

##### Git 删除文件

可以使用以下命令将文件从暂存区和工作区中删除：

```
git rm <filename>
```

如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 `-f`：

```
git rm -f <filename>
```

如果想把文件从暂存区域移除，但仍然希望保留在当前工作目录中，换句话说，仅是从跟踪清单中删除，使用 `--cached` 选项即可：

```
git rm --cached <filename>
```

可以使用以下命令进行递归删除，即如果后面跟的是一个目录做为参数，则会递归删除整个目录中的所有子目录和文件：

```
git rm –r * 
```

进入某个目录中，执行此语句，就会删除该目录下的所有文件和子目录。

##### Git 取消修改

取消修改有三种情况：

**1）未使用 git add 将修改文件添加到暂存区**这种情况下，可以使用以下命令来撤销所有还没有加入到缓存区的修改：

```
git checkout -- <filename>
```

需要注意，此文件不会删除新建的文件，因为新建的文件还没加入到Git管理系统重，所以对Git来说事未知的，需要手动删除。

**2）已使用 git add 将修改文件添加到暂存区，未使用 git commit 提交缓存**这种情况下，相当于撤销了 git add 命令对于文件修改的缓存：

```
git reset HEAD <filename>
```

上面的命令可以撤销指定文件的缓存，要想放弃所有文件的缓存，可以执行以下命令：

```
git reset HEAD
```

需要注意，在使用此命令后，本地的修改并不会消失，而会回到第一种情况。要想撤销本地的修改，执行第一种情况中的命令即可。

除此之外，还可以指定返回到N次提交之前的阶段，执行以下命令即可：

```
git reset HEAD~N
```

这样就能退回到n个版本之前，同样不会修改本地文件的内容，这些新的内容会变成未更新到缓存区的状态。

**3）已使用 git commit 提交缓存**这种情况下，可以使用以下命令来回退到上一次 commit 的状态：

```
git reset --hard HEAD^
```

也可以使用以下命令来回退到任意版本：

```
git reset --hard <commit_id>
```

注意，使用 git log 命令来查看 git 提交历史和 commit id。

##### Git 恢复删除内容

这是一个很重要的命令，假如你回退到某个旧版本，现在想恢复到新版本，又找不到新版本的commit id怎么办？**Git**提供了下面的命令用来记录每一次命令：

```
git reflog show HEADgit reflog
```

执行之后输出如下：

![1712415614475](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712415614475.png)

可以看到，最左侧黄色字体就是修改的commit id，根据这个id就可以将代码恢复到对应节点位置。HEAD@{n}表示HEAD更改历史记录，最近的操作在上面。

假如需要把代码回退到`HEAD@{5}`处，可以执行以下命令：

```
git reset --hard HEAD@{5}
```

或者执行下面的命令：

```
git reset --hard 8a0fd74
```

需要注意，如果有任何本地修改，该命令也会将其销毁，因此在`reset`之前建议使用`stash`将本地修改储存。

#### 日志记录

##### Git 基础日志

可以使用以下命令来查看分支的历史提交信息：

```
git log
```

这是其最基础的用法，输出如下：

![1712415951206](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712415951206.png)

可以看到，终端上输出了该分支近期的提交记录，它包含了所有贡献者的提交。

##### Git 按作者查看

如果想只看某个人的提交，可以添加过滤条件：

```
git log --author="username"
```

当然也可以搜索多个作者的提交信息，只需要在用|分隔用户名即可，注意需要使用\来对|进行转义：

```
git log --author="username1\|usernmae2"
```

这里列出的是每次提交的详细信息，如果指向看到每个提交的概要，可以在命令中添加`--oneline`标志：

```
git log --author="username" --oneline
```

##### Git 按时间查看

除了可以按照作者来查看日志之外，还可以按照时间查看日志。可以查看某个时间之前的日志，也可以查看某个日期之后的日志：

```
//某个日期之后git log --since=<date>git log --after=<date>//某个日期之前git log --until=<date>git log --before=<date>
```

如果想查看某个具体时间区间之间的日志，可以组合以上参数：

```
git log --since="2022.05.15" --until="2022.05.20"
```

##### Git 按文件查看

如果我们想查看某个文件都在哪些提交中修改了内容，也是可以的。使用以下命令即可：

```
git log -- <path>
```

比如查看`README.md`文件的修改记录：

![1712416001508](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712416001508.png)

##### Git 按合并查看

在历史提交中可能会有很多次合并的提交记录，想要只查看代码合并的记录，可以执行以下命令：

```
git log --merges
```

如果想查看非合并操作的操作记录，可以执行以下命令：

```
git log --no-merges
```

##### Git 按分支查看

可以按照分支查看日志，如果想查看`test`分支比`master`分支多提交了哪些内容，就可以执行以下命令：

```
git log master..test
```

相反，如果想看`master`分支比`test`分支多提交了哪些内容，就可以执行以下命令：

```
git log test..master
```

##### Git 美化日志

git log命令可以用来查看提交历史，此命令的问题在于，随着项目复杂性的增加，输出变得越来越难阅读。可以使用以下命令来美化日志的输出：

```
git log --graph --oneline --decorate
```

输出结果如下，这样就能看到更简洁的细分以及不同分支如何连接在一起：

![1712416087384](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712416087384.png)

##### Git 其他标志

上面我们提到了，可以使用`--oneline`标志来简化日志的输出：

```
git log --oneline
```

可以使用`--stat`标志来简要显示文件增改行数统计，每个提交都列出了修改过的文件，以及其中添加和移除的行数，并在最后列出所有增减行数小计：

```
git log --stat
```

可以添加`-N`标志来仅显示最近N次的提交，其中`N`是一个正整数，例如查看最近三次提交：

```
git log -3
```

可以使用`-p`标志来展开显示每次提交的内容差异对比：

```
git log -p
```

注意，以上这些命令标识符都可以组合使用。

#### 差异对比

git diff 命令可以用来比较文件的不同，即比较文件在**暂存区**和**工作区**的差异。

##### （1）未缓存改动

当工作区有改动，暂存区为空时， diff对比的是工作区与最后一次commit提交的共同文件；当工作区有改动，暂存区不为空时，diff对比的是工作区与暂存区的共同文件。

##### （2）已缓存改动

当已缓存改动时，可以使用以下任一命令来显示暂存区（已add但未commit文件）和最后一次commit(HEAD)之间的所有不相同文件的差异对比：

```
git diff --cachedgit diff --staged
```

##### （3）已缓存和未缓存改动

可以使用以下命令来显示工作目录(已修改但未add文件)和暂存区(已add但未commit文件)与最后一次commit之间的的所有不相同文件的差异对比：

```
git diff HEAD
```

##### （4）不同分支差异

可以使用以下命令来比较两个分支上最后 commit 的内容的差别：

```
git diff <分支名1> <分支名2>
```

这样就可以显示出两个分支的详细差异，如果只是想看有哪些文件存在差异，可以在命令中添加`--stat`标志，这样就不会显示每个文件的内容的详细对比：

```
git diff <分支名1> <分支名2> --stat
```

#### 定位问题

git bisect 可以用来查找哪一次代码提交引入了错误。它的原理很简单就是将代码提交的历史使用二分法来缩小出问题的代替提交范围，确定问题出在前半部分还是后半部分，不断执行这个过程，直到找到引入问题的那一次提交。

其命令合适如下：

```
git bisect start <end> <start>
```

其中end就是最近的提交，start就是最开始的提交。假如第一次的提交的 commit id为685f868，总共有21次提交。那么执行以下命令，从第一次提交到最近一次提交：

```
git bisect start HEAD 685f868
```

执行完之后，验证那个问题是否存在，如果发现问题不存在了，就执行以下命令来标识第11次提交是没问题的：

```
git bisect good
```

这样就说明前半段是没有问题的，问题出在后半段，也就是第11-21次提交中。这时再去刷新浏览器，如果问题出现了，使用以下命令来标记：

```
git bisect bad
```

这样就说明第11-16次提交是有问题的。继续重复上面的步骤，直到成功找出那一次提交位为止，这时Git就会给出如下的提示：

```
c8ad045 is the first bad commit
```

这时就确认了是这一次提交导致的问题，可以去查看时那些修改导致的问题。

之后就可以使用以下命令退出错误查找过程，回到最近一次代码提交：

```
git bisect reset
```

#### 标签操作

标签指的是**某个分支某个特定时间点的状态**，通过标签可以很方便的了解到标记时的状态。

标签有两种类型 ：

- **轻量标签 ：** 只是某个commit 的引用，可以理解为是一个commit的别名；
- **附注标签 ：** 存储在Git仓库中的一个完整对象，包含打标签者的名字、电子邮件地址、日期时间 以及其他的标签信息。它是可以被校验的，可以使用 GNU Privacy Guard (GPG) 签名并验证。

##### Git 展示标签

可以使用以下命令来获取所有标签：

```
git tag
```

它会列出所有标签的名称：

![1712415751476](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712415751476.png)

可以使用以下命令来查看某一个标签的详细信息：

```
git show <tag_name>
```

![1712415784313](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712415784313.png)

还可以根据条件来显示标签，比如列出以`v1.`开头的所有tag：

```
git tag -l "v1."
```

##### Git 创建标签

可以使用以下命令在本地创建新标签：

```
git tag <tag_name>
```

例如：

```
git tag v1.0.0
```

通常遵循的命名模式如下：

```
v<major>.<minor>.<patch>
```

- major（主版本号）：重大变化
- minor（次要版本号）：版本与先前版本兼容
- patch（补丁号）：bug修复

除此之外，我们还可以为特定的commit创建标签，其命令格式如下：

```
git tag <tagname> <commit_sha>
```

以上面的的形式创建的标签都属于轻量标签，下面来看看如何创建一个附注标签。

在创建标签时，可以添加一个`-a`标志以创建一个带备注的标签，备注信息使用`-m message`来指定：

```
git tag -a <tagname> -m "<message>"
```

##### Git 推送标签

标签创建完成之后就可以使用以下命令将其推送到远程仓库：

```
git push origin --tags
```

以上命令会将本地所有tag都推送到远程仓库。如果想推送指定标签，可以执行以下命令：

```
git push origin <tagname>
```

##### Git 切换标签

可以使用以下命令来切换标签：

```
git checkout <tagname>
```

##### Git 删除标签

可以使用以下命令来删除本地仓库指定标签：

```
git tag -d <tagname>
```

可以使用以下命令来删除远程仓库指定标签：

```
git push origin :refs/tags/<tagname>
```

也可以使用以下命令来删除远程仓库的指定标签：

```
git push origin --delete <tagname>
```

##### Git 拉取标签

可以使用以下命令来将远程仓库的标签拉取（同步）到当前分支：

```
git fetch --tags
```

##### Git 检出标签

检出标签实际上就是在标签的基础上进行其他开发或操作。需要以标签指定的版本为基础版本，新建一个分支，继续其他的操作。执行以下命令即可：

```
git checkout -b <branch> <tagname>
```

#### 标签tag

```js
gitLab...网站上,点击[标签],[新建标签],填写[标签名],[创建标签]

git tag // 列出当前仓库的所有标签
git tag -n // 列出当前仓库的所有标签及说明
git tag -l "1.0.*" // 搜索符合条件的标签
git show v1.0.1 //查看标签信息

// 创建标签
git tag [tagName] // 创建标签
git tag -a [tagName] -m "指定说明文字" // 创建带有说明的标签

// 给指定的commit打标签
# 找历史提交的 commit id
git log --pretty=oneline --abbrev-commit
# 给指定的commit id打标签
git tag -a [tagName] commitID // git tag -a "v1.0.2" 9fbc3d0

// 删除本地标签
git tag -d [tagName] // git tag -d v1.0.2
// 删除远程标签
git tag -d v1.0.2
git push origin :refs/tags/v1.0.2
// git push origin --delete tag Git1.7版本后的 删除远程标签

// 本地标签推送到远程
git push [remote] [tagName] // git push origin v1.0.2 // 推送指定标签
git push [remote] --tags// git push origin --tags // 一次性推送全部未推送到远程的本地标签

// 重命名tag
// 1. 删除原有tag,重新添加
git tag -d 
git tag -a -m "xxx"
// 2. 强制替换,在删除原有tag
git tag -f
git tag -d

// 新建一个分支,指向某个tag
git checkout -b [branchName] [tagName]
```

#### Git遇到的问题

##### 1. Git 提交了多个 commit 到远程，想要撤回

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps51.jpg) 

Git log 就可以看到提交到远程的commit 已经撤回了

##### 3. **merge 后Git pull 报错**

使用git pull 指令时报错：error: You have not concluded your merge (MERGE_HEAD exists).

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps52.jpg)

错误：您尚未结束合并（merge_HEAD存在）。

提示：请在合并之前提交您的更改。

致命：由于未完成合并而退出。

**(1)** **解决方法****1****：放弃本次merge操作，然后重新pull代码，手动修改冲突代码，合并上传。**

git merge --abort   // 终止合并

git reset --merge   // 重置合并

git pull			// 重新拉取代码

**(2)** **解决办法2：删除 index.lock 文件（本人亲测）**

找到 .git 文件中的 index.lock 文件 删除，重新pull

##### 3. 配置git 公钥、秘钥

在git终端上输入 以下指令  

ssh-keygen -t rsa -c "2973353245@qq.com" 

在c盘 、用户 生成 .ssh 生成 两个文件 

id_rsa.pub 公钥 复制内容 

 

在 github中 setting 设置 公钥   

new SSH key  

黏贴进去 

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps59.jpg)![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps60.jpg) 

##### 4. **报错问题**

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps61.jpg) 

https://blog.csdn.net/dxy1128/article/details/106363962 

 1.$ ssh-keygen -t rsa -C “XXXXXXX@XXX.com” 

 后面改成你的gitee注册或绑定的邮箱。 

 会有要你输入文件位置和两次密码，不用输入，直接回车回车回车就行。  

 \2. $ ssh-agent -s  

 3.把ssh密钥添加到码云 

打开C盘–>用户–>你的用户名–>找到.ssh文件夹。找到id_rsa.pub（日过有多个用最新的那个），用记事本打开，复制整个文本粘贴到gitee（点头像，进入gitee设置面板，SSH设置，将复制的文本粘贴到公钥，标题会自动生成，然后点击添加，根据提示输入密码就可以了。） 

\4. $ [ssh](https://so.csdn.net/so/search?q=ssh&spm=1001.2101.3001.7020) -T git@gitee.com  

 \5. 然后再push就OK啦！解决！ 

### 工具

#### GitLens

GitLens  是一个VS Code插件，可以用来查看项目的提交记录、文件修改记录、显示每行代码的提交记录等。通过丰富的可视化和强大的比较命令**获得有价值的见解**。

![1712416285129](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712416285129.png)

#### Git History

Git History 是一个VS Code插件，增强了Git 的功能，它可以用来查看日志信息，查看和搜索历史，进行分支对比、提交对比，跨提交对比文件等。

![1712416375974](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712416375974.png)

#### 图形化工具

##### 1. **Sourcetree**  

 https://www.cnblogs.com/gongchixin/articles/10708923.html 

  

  \1. Sourcetree 出现错误提示git -c diff.mnemonicprefix=false -c core.quotepath=false fetch origin 

具体表现为：sourcetree无法和gitlab远程仓库进行交互，但使用本地cmd，可以使用git命令和远程仓库交互  

通过各种账户、秘钥等操作，都无法解决该问题  

具体信息如下：    

解决方式：  

点击 工具–>选项  

打开如图所示弹窗，将默认的PuTTY/Plink更改为OpenSSH   

\2. 输错密码后怎么办  

控制面板 --用户账户 -- 凭据管理器 -- windows凭据 -- 找到gitee相关的账户 删掉  

\3. sourcetree 用户密码输入错误后，不出再次输入用户密码的弹窗  

在c：/用户/appdata/atlassian/   userhosts 和 passwd 文件 的内容 清空 

然后 sourcetree 中在提 拉 推  就会出现  输入用户密码的弹窗 