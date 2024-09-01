# Git 基础操作：轻松掌握常用命令

**“** 在开发过程中，掌握 Git 的常用操作命令是提高工作效率的关键。本章将详细介绍 Git 中最常用的命令，如 git add、git commit、git pull 和 git push。你将学习如何处理文件暂存、提交、更改同步及版本历史查看等核心任务。无论你是 Git 新手还是有经验的开发者，这些操作都是你日常工作的基础。**”**

01

# **暂存：git add**

在仓库里刚新建的文件是不会被跟踪起来的，比如我们使用`git status`就能查看到文件的状态。
![1724505175714](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724505175714.png)
需要使用`git add`才可以把本地修改的数据暂存到暂存区。暂存区的作用：像 SVN 这种没有暂存区概念的版本控制，可能会产生很多无意义的提交，暂存区可以先将一些修改暂存一下，后面再统一提交到仓库，从而减少提交次数。

基本用法：

```
git add <path>
```

通过`git add <path>`的方式把`path`目录下的所有文件添加到`git`的暂存区，当然这些文件不包含已经被删除的文件。
示例：

```
# 将所有修改添加到暂存区
git add .  

# 将以.cpp结尾的文件的所有修改添加到暂存区
git add *.cpp   

# 将所有以Hello开头的文件的修改添加到暂存区，例如: helloWorld.txt,hello.h,helloGit.md ...
git add hello*   

# 将以hello开头后面只有一位的文件提交到暂存区 
# 例如:hello1.txt,helloA.cpp 如果是helloGit.txt和hello.cxx是不会被添加的。
git add hello?.*
```

`git add` 是把文件添加到暂存区，那如果想从暂存区删除呢？可以使用`git rm -f` 或者 `git rm –cached `把文件从暂存区里移除，这个移除并不是把代码文件从磁盘上删除了，只是说不被`git`管理了而已。
![1724505195709](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724505195709.png)



02

# **提交：git commit**

`git add` 只是把文件添加到暂存区而已，并没有真正跟踪起来，需要使用`git commit`命令提交到本地仓库才能真正被`git`跟踪记录，`git commit`命令的用法如下：
![1724505277286](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724505277286.png)
示例：

```
#把暂存区和当前已被跟踪的文件的所有的修改提交到仓库里，-m参数指定了此次提交的message内容
git commit -a -m "initial commit" .

# 提交Makefile和Logger.cpp的修改
git commit Makefile Logger.cpp –m "修改编译错误，添加了对log4cpp库的依赖" 
```

![1724505292440](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724505292440.png)



03

# **拉取、拉取合并** 

**拉取**（`git fetch`）：`fetch`是拉取的意思，`git fetch`只将远端仓库数据**拉取到本地仓库**，主要是 **将远程仓库所包含分支的最新`commit-id`记录到本地文件**。比如，远端的数据比本地多两个版本，`fetch`会将最新版本的版本ID写到本地仓库，但是，远端的**文件修改并没有拉取到工作区（workspace）**，它只是拉取最近提交的信息出来，通过这个可以让我们知道本地比远端落后几个版本。

`git fetch`通常很少去使用，因为在实际使用中会使用一个有GUI的客户端工具，并不需要敲命令。这里介绍`git fetch`命令以及其他命令主要是为了了解 Git 的工作流程。

**拉取合并**：`git pull`直接将数据拉取到工作区（workspace）。它主要由两部分构成：

- git fetch：先拉取，看一下本地仓库落后多少个版本信息。
- git merge ：将数据拉取到工作区。

也就是说，别人修改的代码，我们可以先git fetch到本地仓库，然后git merge拉取到工作区；也可以通过一个命令git pull一起完成这两个操作。



04

# **推送：git push**

`git push` 用于将本地仓库中的更改推送到远程仓库。这个命令将本地分支的提交（commits）上传到远程仓库，从而使其他协作者能够看到并合并这些更改。

基本语法：

```
git push [<remote>] [<branch>]
```

- `<remote>`: 远程仓库的名字，通常是 `origin`（默认远程仓库的名字）。
- `<branch>`: 想推送的本地分支名。

示例：

1. 推送到默认远程仓库（origin）和当前分支：

   ```
   git push
   ```

   如果当前分支已经配置了上游分支（upstream branch），这个命令会将更改推送到默认远程仓库的对应分支。

2. 推送到指定的远程仓库和分支：

   ```
   git push origin main
   ```

   将本地的 main 分支推送到远程的 main 分支。

3. 推送所有本地分支：

   ```
   git push --all
   ```

   将所有本地分支推送到远程仓库。

4. 推送所有标签（tags）：

   ```
   git push --tags
   ```

   将所有本地标签推送到远程仓库。

常见选项：

- -u 或 --set-upstream：将本地分支与远程分支关联起来，后续可以只用 git push 或 git pull 不指定分支。

  ```
  git push -u origin feature-branch
  ```

- --force 或 -f：强制推送，覆盖远程仓库的历史记录。注意使用这个选项时要非常小心，因为这可能会导致数据丢失。

  ```
  git push --force
  ```

- --force-with-lease：在强制推送时确保不会覆盖别人推送的更改。相对比 --force 更安全一些。

  ```
  git push --force-with-lease
  ```

**错误处理：**

- **`rejected` 错误**：通常是因为远程分支比本地分支有更新，可能需要先拉取远程更改并解决冲突。

- non-fast-forward 错误：通常需要进行合并或变基操作。

  ```
  git pull --rebase
  git push
  ```



05

# **查看状态：git status** 

`git status` 是 Git 中一个非常有用的命令，用于显示当前工作目录和暂存区的状态。这有助于了解哪些文件被修改了、哪些文件被暂存了、以及哪些文件是未跟踪的。

基本语法：

```
git status
```

执行 `git status` 后，会看到以下几类信息：

1. 当前分支信息：显示你当前所在的分支以及它与远程分支的关系（例如，是否领先或落后于远程分支）。

   ```
   On branch main
   Your branch is up to date with 'origin/main'.
   ```

2. 暂存区状态：显示哪些文件被暂存（即已准备好进行提交）。

   ```
   Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)
     
     new file:   file1.txt
     modified:   file2.txt
   ```

3. 工作目录状态：显示哪些文件被修改但尚未暂存。

   ```
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
     
     modified:   file3.txt
   ```

4. 未跟踪的文件：显示哪些文件存在于工作目录中，但 Git 还没有跟踪它们。

   ```
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
     
     file4.txt
   ```

常见选项：

- -s 或 --short：以简短的格式显示状态。

  ```
  git status -s
  ```

  输出类似于：

  ```
  A  file1.txt
  M  file2.txt
  M  file3.txt
  ?? file4.txt
  ```

  这里的 `A` 表示新文件已暂存，`M` 表示修改已暂存，`??` 表示未跟踪的文件。

- -b 或 --branch：显示分支信息。

  ```
  git status -b
  ```

  输出类似于：

  ```
  On branch main
  Your branch is up to date with 'origin/main'.
  ```

- --porcelain：以机器可读的格式输出状态信息，通常用于脚本。

  ```
  git status --porcelain
  ```

如果工作目录中有很多未跟踪的文件或修改，可以考虑使用 `git clean` 或 `git restore` 命令来清理。

06

# **查看历史：git log** 

`git log` 是 Git 中一个非常重要的命令，用于查看提交历史。它显示了当前分支的提交记录，帮助了解代码的演变过程。

基本语法：

```
git log [options] [<revision range>] [--] [<path>...]
```

- `<revision range>`: 可以指定一个或多个提交范围。
- `<path>`: 仅显示特定路径的提交记录。

基本用法：

1. 查看提交历史：

   ```
   git log
   ```

   这将显示当前分支的所有提交记录，包括提交的哈希值、作者、日期和提交信息。

2. 查看简洁的提交历史：

   ```
   git log --oneline
   ```

   以简洁的一行格式显示提交记录，每个提交显示其简短的哈希值和提交信息。

3. 查看特定文件的提交历史：

   ```
   git log -- <file>
   ```

   显示对指定文件的所有提交记录。

4. 查看某个分支的提交历史：

   ```
   git log branch-name
   ```

   显示指定分支的提交记录。

07

# **引用日志：git reflog** 

`git reflog` 是 Git 中一个非常重要的命令，用于查看和管理引用日志（reflog）。引用日志记录了对 Git 引用（如分支、HEAD）的所有修改历史，包括提交、合并、重置、移动分支等操作。它在恢复丢失的提交、调试和审计历史方面非常有用。

基本语法：

```
git reflog [options]
```

这将显示 HEAD 的所有历史记录，包括提交、重置、合并等操作。输出内容包括操作编号（reflog index）、提交哈希、操作类型和消息。

**也可以查看特定操作的引用日志：**

```
git reflog show HEAD@{<n>}
```

这里的 `<n>` 是 `reflog` 的索引。例如，`HEAD@{1}` 表示上一个 `HEAD` 状态。可以用来查看指定历史状态的详细信息。

<iframe src="https://file.daihuo.qq.com/mp_cps_goods_card/v111/index.html" frameborder="0" scrolling="no" class="iframe_ad_container" style="width: 677px; height: 927px; border: none; box-sizing: border-box; display: block;"></iframe>



08

# **总结** 



在这一章中，我们详细介绍了 Git 的一些常用操作命令，包括 `git add``git commit``git pull``git push``git status``git log``git reflog`



