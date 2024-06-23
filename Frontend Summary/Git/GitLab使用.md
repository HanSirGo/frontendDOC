# GitLab使用

## GitLab使用[管理员]

> 一般这些配置是需要公司的管理员去配置的，初级开发人员是没有这个权限的。

### 修改显示主题为中文

在主页面左上角用户头像上点击，选择`Edit profile`，效果如下图：

![1719057992113](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719057992113.png)



选择菜单，滚动到下面的组的，效果如下图：

![1719058024558](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719058024558.png)

设置语言为`Chinese,Simplified-简体中文(98%translated)`，效果如下图：

![1719058041418](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719058041418.png)

点击`Save changes`保存修改，效果如下图：

![1719058060576](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719058060576.png)

修改语言为中文后，需要刷新网页才能看到页面全部修改为中文了，效果如下图：

![1719058092752](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719058092752.png)

### 取消用户自动注册功能

点击`管理中心` -> `仪表盘` -> `己启用注册功能` 去掉勾选，然后拉到下面点击`保存更改`，效果如下图：

![1719058111330](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719058111330.png)



![1719058125021](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719058125021.png)



!![1719060548570](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060548570.png)

### 创建用户

创建用户cxypa01，点击`仪表盘` -> `新建用户`，效果如下图：

![1719060569646](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060569646.png)

填写用户账号信息，点击`创建用户`，效果如下图：

![1719060637756](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060637756.png)

创建后点击`编辑`，效果如下图：

![1719060652804](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060652804.png)

添加用户的密码，效果如下图：

![1719060712452](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060712452.png)

### 创建组

`群组`可有理解为企业中的`开发小组`，每个小组可能负责固定的几个项目。
点击`仪表盘` -> `新建群组`，效果如下图：

![1719060733272](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060733272.png)

输入相关信息，点击`创建群组`，效果如下图：

![1719060751980](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060751980.png)

### 将用户添加到组中

点击`群组`，点击群组名`testGroup`，进入之前创建的组中，效果如下图：

![1719060779481](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060779481.png)

点击`管理权限`，效果如下图：

![1719060809776](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060809776.png)

点击`邀请成员`，效果如下图：

![1719060824638](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060824638.png)

在邀请成员对话框中输入相关信息，效果如下图：

![1719060839475](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060839475.png)

添加群组成员后，效果如下图：

![1719060854777](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060854777.png)

### 给用户组创建项目

点击`testGroup`群组，点击右侧`创建新项目`，效果如下图：

![1719060882401](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060882401.png)

点击`创建空白项目`，效果如下图：

![1719060899722](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060899722.png)

在`创建空白项目`中输入项目名称，不要勾选`使用自述文件初始化仓库`，效果如下图：

![1719060911433](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060911433.png)

完成项目创建，效果如下图：

![1719060922579](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060922579.png)

## SSH 协议

![1719060940656](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060940656.png)


GitLab 可以通过 HTTP 的方式可以上传和下载代码，这种方式可以用户的账号和密码。此方式在这里就不过多介绍了，下面将介绍使用 SSH 协议来操作 GitLab 上的项目。

SSH 为 Secure Shell （安全外壳协议）的缩写，由 IETF 的网络小组（Network Working Group）所制定。SSH 是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议。利用 SSH 协议可以有效防止远程管理过程中的信息泄露问题。

### 基于密匙的安全验证

使用 SSH 协议通信时，推荐使用基于密钥的验证方式。你必须为自己创建一对密匙，**并把公用密匙放在需要访问的服务器上。**如果你要连接到 SSH 服务器上，客户端软件就会向服务器发出请求，请求用你的密匙进行安全验证。服务器收到请求之后，先在该服务器上你的主目录下寻找你的公用密匙，然后把它和你发送过来的公用密匙进行比较。如果两个密匙一致，服务器就用公用密匙加密“质询”（challenge）并把它发送给客户端软件。客户端软件收到“质询”之后就可以用你的私人密匙解密再把它发送给服务器。

### SSH密钥生成

密钥生成的方式有很多种，常见的JDK的工具或者是GitBash等。本教程使用GitBash。

1. 鼠标右键打开GitBash客户端，效果如下图：

   ![1719060957551](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060957551.png)

2. 在GitBash执行命令，生命公钥和私钥，效果如下图：

```
ssh-keygen -t rsa
```

![1719060973962](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060973962.png)


执行命令完成后，在window本地用户.ssh目录（C:\Users\用户名.ssh中），生成如下名称的公钥和私钥:

![1719060996273](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719060996273.png)

### SSH密钥配置

密钥生成后需要在GitLab上配置密钥本地才可以顺利访问。
SSH密钥配置操作步骤如下图：

![1719061086318](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061086318.png)

![1719061103939](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061103939.png)

![1719061113337](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061113337.png)

设置成功后， 我们就可以使用SSH的形式上传和下载代码了。

![1719061125877](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061125877.png)

![1719061135195](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061135195.png)

![1719061150730](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061150730.png)

在windows中使用`Git Bash`命令行测试是否能够使用ssh连接服务器，命令如下：

```
ssh -T Git@192.168.100.132 -p 1024
```

效果：

```
$ ssh -T Git@192.168.100.132 -p 1024
The authenticity of host '[192.168.100.132]:1024 ([192.168.100.132]:1024)' can't be established.
ED25519 key fingerprint is SHA256:yBstIZL6RmVX65G9yUcBjYQET8GuIq0z3gpf2PmhPhU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[192.168.100.132]:1024' (ED25519) to the list of known hosts.
Welcome to GitLab, @root!
```

看到`Welcome to GitLab, @root!`说明成功了!

## 上传项目至GitLab[管理员]

使用 IDEA 中把项目上传到 GitLab 中，无需其他的插件。这种方式不仅可以向 GitLab 中上传，也可以向 Gitee 和 Github 中上传。

1. 点击 VCS，创建本地仓库`Create Git Repository`

![1719061202540](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061202540.png)

2. 选择项目的根目录，作为 Git 本地仓库的根资源库。

![1719061225903](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061225903.png)

3. 向项目的根路径下添加 `.gitignore` 文件忽略不要Git仓库管理的文件

![1719061245343](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061245343.png)

![1719061259141](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061259141.png)

4. 选择要提交到本地仓库中的文件

![1719061303196](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061303196.png)

5. 将项目上传到远程仓库 GitLab 中
   选中项目的根目录，在 Git 菜单栏中选择 Push

![1719061324443](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061324443.png)

6. 点击 `Define remote` 在弹出窗口中的 URL 里填写之前的项目地址
   注意：保证远程仓库是一个新的空仓库，否则提交失败。

![1719061368695](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061368695.png)

7. 点击 Push 进行提交

![1719061386532](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061386532.png)

8. 检查 Git 中是否已经上传到远端仓库地址中

![1719061445540](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061445540.png)

9. 远端仓库中上传后项目的内容

![1719061465583](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061465583.png)


以上将自己的项目上传到了 GitLab 组中的项目中，只有本组的人员才可以看到此项目中的内容。

### 新建分支

![1719061488176](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061488176.png)

![1719061528498](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061528498.png)

### 上传分支

![1719061554258](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061554258.png)

![1719061568641](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061568641.png)

## 模拟新员工到公司[新员工]

模拟新员工到公司，在GitLab上删除之前的SSH秘钥，退出的root用户。

![1719061598465](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061598465.png)

### 使用新建的用户登录GitLab[开发人员]

![1719061631142](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061631142.png)


第一次登录后会提示修改密码，效果如下图：

![1719061645006](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061645006.png)

### SSH密钥生成[新员工]

在`Git bash` 执行下面命令，生成公钥和私钥。

```
ssh-keygen -t rsa
```

### SSH密钥配置[新员工]

生成密钥后，需要在 GitLab 上配置密钥，本地才可以顺利访问 GitLab。操作如下图：

![1719061660713](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061660713.png)

![1719061672266](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061672266.png)

![1719061681702](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061681702.png)

### 使用IDEA从GitLab拉取代码[新员工]

在IDEA中关闭项目，回到欢迎界面，选择`Get from VCS`，效果如下图：

![1719061722619](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061722619.png)

到GitLab上复制项目的地址，效果如下图：

![1719061731912](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061731912.png)

输入GitLab对应项目的地址，选择本地保存项目的路径，点击`Clone`，效果如下图：

![1719061746246](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061746246.png)

点击`Trust Project`，效果如下图：

![1719061768836](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061768836.png)


可以看到代码拉取到本地了，效果如下图：

![1719061781607](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061781607.png)


如果使用HTTP协议拉取代码，第一次拉取代码会显示需要输出账号和密码，输入GitLab的账号和密码。

![1719061791301](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061791301.png)

### 把远程分支拉到本地[新员工]

刚从远程仓库把代码拉取到本地，本地只有一个master分支。需要把远程分支拉取到本地，效果如下图：

![1719061804463](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061804463.png)

点击`远程分支名`，再选择`Checkout`，就可以把远程分支拉取到本地，效果如下图：

![1719061823654](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061823654.png)

![1719061834821](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061834821.png)

### 使用IDEA创建新分支开发[新员工]

假设`张三`接到了开发`订单功能`的任务，张三需要创建一个新分支`zhangsan_order`分支开发订单功能，这样不会影响其他的人开发。当订单功能开发完成后就需要合并到`dev`分支。
张三创建一个新分支`zhangsan_order`分支开发订单功能，操作如下图：

![1719061884693](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061884693.png)

![1719061897626](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061897626.png)

![1719061906765](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061906765.png)

![1719061920478](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061920478.png)

在`zhangsan_order`分支提交代码，操作如下图：

![1719061938303](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061938303.png)

张三把订单功能`zhangsan_order`分支合并到`dev`分支，操作如下图：

![1719061953638](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061953638.png)

![1719061963245](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061963245.png)

`zhangsan_order`分支合并到`dev`分支后的效果：

![1719061981669](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061981669.png)


推送分支到远程仓库。

![1719061993671](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719061993671.png)

在远程仓库的`dev`分支可以看到订单功能的代码。

![1719062004019](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062004019.png)

## Git开发常见问题

### commit前回滚[新员工必会]

假设用户添加了一行代码，但是还没有提交到本地仓库。

![1719062019449](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062019449.png)

在commit窗口中右键选择要回滚的文件，点击`Git`，再点击`Rollback`，操作如下图：

![1719062042508](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062042508.png)

![1719062053404](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062053404.png)

![1719062061851](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062061851.png)
回滚后，多添加的但是未提交的代码没有了。

commit后回滚有两种方式：`Undo Commmit`和`Revert Commit`。
`Undo Commmit`：撤销上次提交，保留修改的内容到新的changelist中，Git log中无任何提交信息。
`Revert Commit`：直接回滚到上个版本，不保留修改的内容，在Git log中会留下上次commit和本次revert的信息。

#### `Undo Commmit`操作

在Git log的提交日志上右键，选择`Undo Commit`，操作如下图：

![1719062076540](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062076540.png)


点击OK

![1719062116625](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062116625.png)

`Undo Commmit`操作后，在Git log中就没有了这次提交。

![1719062130542](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062130542.png)

但是代码还是存在的，需要`Rollback`回滚代码，操作如下图：

![1719062189286](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062189286.png)

![1719062204727](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062204727.png)

代码还原了，Git log也没有了，效果如下图：

![1719062217012](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062217012.png)

![1719062228210](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062228210.png)

#### `Revert Commmit`操作

先提交测试代码，效果如下图：

![1719062238841](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062238841.png)

在Git log提交日志上右键，选择`Revert Commit`，操作如下图：

![1719062249807](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062249807.png)

提交的代码撤回了，但是Git log提交日志中多了`Revert"commit后回滚测试2！提交"`，效果如下图：

![1719062261087](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062261087.png)

### push后回滚[新员工必会]

假设用户不小心push到了服务器，效果如下图：

![1719062271882](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062271882.png)

在Git log提交日志中，选择要撤回的到的提交日志，右键，点击`Reset Current Branch to Here`

![1719062283496](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062283496.png)

在弹出窗中选择soft或者hard。
区别：hard彻底回滚，不保留修改的内容。soft回滚到commit前的状态，修改的内容被保留在changelist中。
这里选择`Soft`，效果如下图：

![1719062294896](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062294896.png)

修改完成之后使用`Force Push`强制推送到远程仓库，操作如下图：

![1719062305379](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062305379.png)

![1719062316949](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062316949.png)

到远程仓库查看，可以看到，之前的提交被覆盖掉了。

![1719062326954](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062326954.png)

如果IDEA的`Force Push`按钮不能点击，则使用Git命令强行覆盖远程分支`Git push origin 分支名 --force`，效果如下：

```
Git push origin master --force
```

## 冲突解决[新员工必会]

两个人在同一个类的同一行添加代码并先后提交到远程仓库。

![1719062338583](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062338583.png)

产生了冲突，解决冲突，操作如下图：

![1719062347785](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062347785.png)

![1719062358979](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062358979.png)

![1719062376639](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062376639.png)

解决冲突后的效果：

![1719062387826](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719062387826.png)

## 推荐阅读

1. # [再也不用担心Linux命令记不住了，Linux必会命令，赶紧收藏，方便以后参考！](http://mp.weixin.qq.com/s?__biz=MzkxOTUyMzEzMw==&mid=2247488076&idx=1&sn=cce4a06c445d83b7317389dd2892882e&chksm=c1a1881ef6d6010866411556d4329d1526a94522c10c00bb89377c81802325cb0a1e9a77bd8c&scene=21#wechat_redirect)

2. # [赶紧收藏，IDEA安装上这46款神级插件，效率直接起飞](http://mp.weixin.qq.com/s?__biz=MzkxOTUyMzEzMw==&mid=2247486894&idx=1&sn=58b2675842cec779c1158df596884968&chksm=c1a197fcf6d61eeabb1d4916bbfc56a1e222c59e316deb5f7618f58c345d4906b48ef3ec3988&scene=21#wechat_redirect)

3. # [SpringBoot葵花宝典！最细SpringBoot教程，赶紧收藏学习！](http://mp.weixin.qq.com/s?__biz=MzkxOTUyMzEzMw==&mid=2247488088&idx=1&sn=03b6e52b21d6163ce044b19443114bee&chksm=c1a1880af6d6011cf3b4442b91389022d8803d09fc9e7263535a9845253cac8d64ab2c6ac4c5&scene=21#wechat_redirect)

4. # [培训班？非科班？不懂算法？十大排序算法汇总，轻松学会算法，赶紧收藏学习！](http://mp.weixin.qq.com/s?__biz=MzkxOTUyMzEzMw==&mid=2247488069&idx=1&sn=e7d2a7979f092b93d85c64e2fc7c2e98&chksm=c1a18817f6d6010155bb24b8e6c8a145ac5af8fd18587273db77d03ba6c46c588d36317af3ea&scene=21#wechat_redirect)

5. # [IDEA永久激活破解免费使用，支持Windows、Mac（超简单的图文保姆教程）](http://mp.weixin.qq.com/s?__biz=MzkxOTUyMzEzMw==&mid=2247485453&idx=1&sn=d9e68a61c0bcc449e602aab6654171b1&chksm=c1a1925ff6d61b4933999466124ebad46fa765ec85e5ea96a6efb691fdad7d931c20f34a519c&scene=21#wechat_redirect)

-------------------- END --------------------

