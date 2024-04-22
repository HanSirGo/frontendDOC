# Git回滚代码

## 前言

在日常的代码回滚中常用的有两种方式`git revert`和`git reset`来进行回滚，这两种分别对应的不同的情况我尽量简单明了的介绍这两个命令都能做些什么，接下来我会从个人仓库新拉个分支从0开始，建两个分支，分别是主分支`master`和开发分支`develop`来进行模拟

## 开始

### reset介绍

> 1、`reset`的作用是当你希望提交的`commit`从历史记录中完全消失就可以用
>
> 2、比如你在`master`分支提交了`A-->B-->C`提交了三个记录，这个时候如果C记录有问题你想回滚到B就可以用`git reset`进行
>
> 3、这个命令大概率的情况都是用在我们主分支的，因为我们上线的分支一般是`master`分支然后从`develop`进行功能开发
>
> 4、开发完成之后将分支合并到`master`，如果在上线之前发现合并的分支用问题可以将`develop`合并过来的分支进行回滚
>
> 5、说白了就是取消`develop`的本次合并
>
> 6、但是有一种情况就是协作开发的时候大家都合并到`master`之后就不能用`reset`强行回滚`commit`因为这样会把其他人的提交记录给冲掉，这时候就可以用`revert`来进行操作我们在下面说

**制造一个分支模拟环境**

1.从你自己的git仓库创建一个新项目之后拉到本地

2.创建一个index.js随便写点东西，之后提交到仓库

![1713609789084](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713609789084.png)

3.我们在终端使用`git log`查看commit可以看到目前只有一个刚才提交的commit

![1713609802122](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713609802122.png)

4.我们从`master`分支迁出一个`develop`分支`git branch develop`，并且切换到该分支 `git checkout develop`

5.在`develop`分支新增一段代码，这个时候develop的commit记录就新增了一条B的记录

![1713609827040](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713609827040.png)

6.在develop分支接着新增一段代码

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713609837121.png)

7.看下develop分支和master分支最新的commit记录对比，可以看到dev分支领先master分支两个commit

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713609855409.png)![1713609871356](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713609871356.png)

> 注意这里有个问题当你进行分支合并的时候，有时候会发现虽然代码不一样但是在进行分支合并的时候就提示代码没有更新，就是因为当前的开发分支的commit记录是落后于要合并的目标分支的，造成这种情况的原因就是reset滥用造成的，所以reset一定要慎用

**操作一下reset来感受一下**

1.我们将`develop`分支的代码合并到`master`，切换到`master`分支 执行`git merge develop`

2.我们在master分支使用`git log`查看commit记录找到B记录，准备回滚这一条，回滚的时候不需要输入全部的commid一般是前7位就够用

![1713609890127](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713609890127.png)

3.重点来了我们使用`git reset 69fde2c`进行回滚，这个时候查看log记录发现最后一条`新增c`记录没有了，这里还有个问题如果直接使用`git push`推送会有以下提示。

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713609901719.png)

> 这是因为本地的记录因为我们的回滚已经落后于仓库的代码了，这个使用需要使用`git push \-f`进行强制提交

4.这个时候master分支就剩下A和B的commit记录了，到这里就是一次完整的reset回滚记录，之后我们还是可以继续正常把develop分支合并到master的

![1713609915825](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713609915825.png)

### revert介绍

> 1、revert的原理是，在当前提交后面，新增一次提交，抵消掉上一次提交导致的所有变化。它不会改变过去的历史，所以是首选方式，没有任何丢失代码的风险
>
> 2、revert可以抵消上一个提交，那么如果想要抵消多个需要执行 `git revert 倒数第一个commit id 倒数第二个commit`
>
> 3、这个就常用于当你提交了一次commit之后发现提交的可能有问题就可以用到revert
>
> 4、还有一种情景是已经有很多人提交过代码，但是想改之前的某一次commit记录又不想影响后面的也可以使用revert，他会把你后面提交的记录都放到工作区只是合并的时候需要注意一点

**我们来模拟一下环境**

1.切到develop分支现在该分支有三个commit记录![1713609931356](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713609931356.png)

2.我们使用rever进行回滚试一下`git revert 16083ce`，如果你也用的是vs code可以看到工作区的变化，并且在控制台可以提交默认的commit

![1713609944970](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713609944970.png)

3.看一下log记录，可以看到新增了一个记录`Revert 新增C`，并且原来的`新增C`还是在的

![1713609983520](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713609983520.png)

### commit记录打tag

> 1、在上线之前我们需要对当前的commit记录打一个tag方便上线的代码有问题可以及时回滚

**我们来介绍一下常用的几个命令**

1.`git tag`列出所有的tag列表

![1713610043110](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713610043110.png)

2.创建一个tag，使用`git tag [name]`,我们新增一个 `git tag 测试4`,在使用`git tag` 查看一下

![1713610058814](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713610058814.png)

3.查看tag对应的commit信息，`git show [tag名字]`，举个例子`git show 测试1`,上线之后如果有问题我们就可以根据 下图的commit id进行代码回滚

![1713610076976](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713610076976.png)

