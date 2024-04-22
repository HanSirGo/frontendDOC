# gitignore的匹配规则

## 一、概述

.gitignore 是一个用于指定哪些文件或目录应该被 Git 忽略的规则文件。当我们指定之后，就不会提交内容到仓库内啦！我们来看看它的匹配规则

### 1.书写规则

每一个匹配规则占据一行。![1713689398366](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689398366.png)不能多个匹配占据一行。

## 二、匹配规则

### 1.直接写名称

```
dist
```

当直接写匹配的名称时，它会匹配当前.gitignore所在项目、文件夹下所有的dist文件夹或文件

![1713689455455](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689455455.png)

可以发现，不管是文件还是文件夹都会被匹配，而且不只是第一层文件夹，文件夹下的文件夹/文件都可以匹配到。

![1713689472043](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689472043.png)

### 2.带斜杠

**1.斜杠在前面**

这个时候，它就会去匹配从.gitignore文件所在的位置出发，去找紧挨着目录层级的文件/文件夹

![1713689528351](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689528351.png)

![1713689543502](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689543502.png)

所以上面图中只会匹配到一个。如果我把.gitignore位置移动，再来看看，它就是这样了。

![1713689562659](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689562659.png)

**2.斜杠在中间**

```
src/dist
```

同样的，它还是会从当前.gitignore所在位置开始去匹配。

![1713689597234](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689597234.png)

![1713689609703](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689609703.png)

**3.斜杠在后面**

```
dist/
```

当斜杠在后面的时候，其实和上面直接写名称没有什么太大区别，主要是斜杠在后面表示匹配文件夹，而不带斜杠时候，是匹配文件夹/文件。

![1713689638899](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689638899.png)

![1713689650051](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689650051.png)

### 3.*

*号匹配的是任意字符，但是斜杠/除外。例如：

![1713689687561](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689687561.png)

![1713689701329](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689701329.png)

这个时候，它匹配的是所有的jpg文件。但是如果我加上斜杠。

```
dist/*.jpg
```

![1713689730911](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689730911.png)

![1713689742544](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689742544.png)

会发现，都没有匹配到，为什么，因为dist下面的jpg图片不存在，可能你看见dist下面有个2.jpg，但是它的完整路径是dist/imgs/2.jpg。所以无法匹配。

如果我就是要匹配，那我怎么办呢？

```
dist/**/*.jpg
```

![1713689766803](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689766803.png)

这样就可以了，为啥，之前我们说过，* *表示匹配所有文件/文件夹。这里就是匹配，dist下面所有的文件夹/文件。

![1713689782177](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689782177.png)

### 4.?

?号表示匹配的是字符。就拿刚才那个来看。

```
dist/**/?.jpg
```

![1713689809608](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689809608.png)

会发现，33.jpg没有被匹配上，为什么，因为33是两个字符，你如果要匹配两个，那就来两个？？。

![1713689833263](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689833263.png)

![1713689860975](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689860975.png)

不过这样之后，一个字符的就无法匹配啦。

### 5.！

当我们有个文件夹里面，基本上所有的文件都要让它被忽略时候。只有某个文件需要提交时候。例如

![1713689888040](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689888040.png)

这里我把a文件夹下所有的都忽略啦，但是a文件夹下面的文件夹除啦config.js我需要提交。那怎么办呢

```
src/**/*.*
!src/**/config.js
```

![1713689924235](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689924235.png)

![1713689935390](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713689935390.png)

这个时候！就用到了，就是用来排除的，也就是非的意思。