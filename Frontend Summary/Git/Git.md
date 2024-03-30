## Git

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

##### Git status

```js
git status // 命令用于检查存储库的状态（例如未跟踪的文件和准备提交的更改）
```

##### Git log

```js
git log // 命令用于显示所有提交的历史记录。它包括提交哈希、作者、日期和提交消息。
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

#### 分支

##### Git 创建分支

```js
git branch <branchname>
git checkout -b <local branchname> // 创建本地分支,并切换到本地分支
git checkout -b <local branchname> <origin/branchname> // 创建本地分支,并切换到本地分支,并且这这种方法创建的本地分支会和远程分支建立映射关系
```

##### Git 切换分支

```js
git checkout <local branchname> // 切换到本地分支
git checkout -b <local branchname> // 创建本地分支,并切换到本地分支
git checkout -b <local branchname> <origin/branchname> // 创建本地分支,并切换到本地分支,并且这这种方法创建的本地分支会和远程分支建立映射关系

git switch 分支; git checkout 分支; // 切换分支

git switch -c 分支; git checkout -b 分支 // 创建分支
```

##### Git 合并分支

```
git add . 暂存缓存区
Git commit -m ‘’ 提交本地仓库
Git branch -a 查看所有分支
Git checkout 远程分支
Git status
Git pull
Git log
Git checkout 本地分支
Git merge 远程分支 （将远程分支合并到本地）
有冲突解决冲突（在分支上有一个表示 MING 有冲突）
Git status
Git commit -m ‘’
Git push  （推送到远程仓库后，在远程仓库中提交自己的分支合并到需要的分支上）
```

##### Git 删除分支

```
Git branch -d branchname 删除本地分支
						删除远程分支
```

##### Git 查看分支

```js
当通过命令：
$ git branch -a  // 查看所有分支:本地&远程
$ git branch -r  // 查看所有分支:远程
$ git branch // 罗列分支

查看不到远程dev分支时，可以通过命令：
$ git fetch  // 进行刷新，然后再通过 
$ git branch -a   // 就可以看到远程分支了。
```

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
git add . // 提交所有的更改到暂存区
```

##### Git 取消暂存

```
git reset <file_name>
```

##### 缓存

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

```
git commit -m '注释' // 提交到本地仓库
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