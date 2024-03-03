## Git

#### Git命令

![1709187444673](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709187444673.png)

```
git add 文件名  追踪这个文件
git status 查看状态 红色没追踪 绿色追踪
git add . 所有的文件
git rm --cached 文件名（可多个） 取消追踪
git checkout -- 文件名 还原成修改前的状态
```

##### Git合并分支

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

##### Git 克隆

```
Git clone url(http地址)
Git clone -b branchname remoteaddr 克隆指定分支 remoteaddr （http地址）
```

##### Git 删除分支

```
Git branch -d branchname 删除本地分支
						删除远程分支
```

##### Git **版本回退**

```
1. Git log 查看commit 提交；复制 版本号

2. 使用 git reset -- hard 版本号 

3. Git push -f origin 分支
```

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps58.jpg)

##### Git 将本地与远程分支关联

**Git 将本地分支与远程分支关联（不同名也可以），并提交**

```
1. 查看本地分支与远程分支是否关联 git branch -vv;有蓝色分支是关联，没有就是没有关联的远程分支
```

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps53.jpg) 

```
2. feature/performance-md 只是一个本地分支；

想要关联就 git branch -u origin/feature/performance

或者 git branch --set-upstream-to=origin/feature/performance
```

​    ![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps54.jpg)

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps55.jpg) 

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps56.jpg) 

```
3. 这样就可以将本地分支的feature/performance-md推送到 远程分支origin/feature/performance，也可以拉去远程分支代码

4. 推送到远程，注意方式不一样，要多一个 " head: "  git push origin HEAD:feature/performance

Git push 时会有提示
```

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps57.jpg) 

 ![1709187760764](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709187760764.png)

##### Git 查看所有分支

```
当通过命令：
$ git branch -a  查看所有分支:本地&远程

查看不到远程dev分支时，可以通过命令：
$ git fetch  进行刷新，然后再通过 
$ git branch -a   ，就可以看到远程分支了。
```

##### Git 查看当前分支是基于哪个分支创建的

```
切换到要查询的分支下
git checkout branchname
使用命令：
git reflog --date=local --all | grep 后面加上要查询的分支名称
```

#### Git 分支规范

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