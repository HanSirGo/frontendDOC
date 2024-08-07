## npm

### npm 

npm 全称为 Node Package Manager，是一个基于 Node.js 的包管理器，也是 Node.js 社区最流行、支持的第三方模块最多的包管理器。它的初衷就是让开发人员更容易分享和重用代码。npm 提供了命令行工具，其主要功能是管理Node.js包，包括安装、更新、删除、查看、搜索、发布等。

npm 最初只是Node.js 的包管理器，但随着前端技术的不断发展，它的定位变成了广义的包管理器，可以实现JavaScript、React、Vue、Gulp、移动开发等包管理，是目前最大、生态最为健全的包管理器。

npm 能解决 Node.js 在模块管理上的很多问题，其常见的应用场景如下：

- 从npm镜像服务器下载第三方模块；
- 从npm镜像服务器下载并安装命令行程序到本地；
- 自己发布模块到npm镜像服务器供他人使用。

npm 不需要单独安装，在安装 Node.js 时，就会连带着一起安装 npm 了。但是安装的 npm 不一定是最新的版本，可以使用以下命令来查看本地 npm 的版本：

```
npm -v
```

这里的 -v 是 --version 的缩写，表示版本。如果想升级 npm 版本，可以使用以下命令：

```
npm install npm@latest -g
```

这里@latest表示最新的版本，-g 是 --global 的缩写，表示全局安装。

除此之外，还可以使用help命令来查看npm帮助：

```
npm 命令 --help
```

比如查看 install 的参数形式：

```
npm install --help
```

其中--help可以简写为-h，上面命令的执行结果如下，可以看到install命令的很多形式：

![1712468016137](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712468016137.png)

常见的npm命令：

| **命令**                                                | **作用**                                                     |
| :------------------------------------------------------ | :----------------------------------------------------------- |
| npm -v                                                  | 查看 npm 版本。                                              |
| npm init                                                | 初始化后会出现一个 package.json 配置文件。可以在后面加上 -y ，快速跳过问答式界面。 |
| npm install                                             | 根据项目中的 package.json 文件自动下载项目所需的全部依赖。   |
| npm install 包名 --save-dev(npm install 包名 -D)        | 安装的包只用于开发环境，不用于生产环境，会出现在 package.json 文件中的 devDependencies 属性中。 |
| npm install 包名 --save(npm install 包名 -S)            | 安装的包需要发布到生产环境的，会出现在 package.json 文件中的 dependencies 属性中。 |
| npm list                                                | 查看当前目录下已安装的 node 包。                             |
| npm list -g                                             | 查看全局已经安装过的 node 包。                               |
| npm --help                                              | 查看 npm 帮助命令。                                          |
| npm update 包名                                         | 更新指定包。                                                 |
| npm uninstall 包名                                      | 卸载指定包。                                                 |
| npm config list                                         | 查看配置信息。                                               |
| npm 指定命令 --help                                     | 查看指定命令的帮助。                                         |
| npm info 指定包名                                       | 查看远程 npm 上指定包的所有版本信息。                        |
| npm config set registry https://registry.npm.taobao.org | 修改包下载源，这里修改为了淘宝镜像。                         |
| npm root                                                | 查看当前包的安装路径。                                       |
| npm root -g                                             | 查看全局的包的安装路径。                                     |
| npm ls 包名                                             | 查看本地安装的指定包及版本信息，没有显示 empty。             |
| npm ls 包名 -g                                          | 查看全局安装的指定包及版本信息，没有显示 empty。             |

说完这些概念，下面就来看看 npm 在使用时有哪些实用的技巧。

### 初始化 package.json

凡是使用npm管理的项目，都需要初始化一个package.json文件。

可以使用以下命令来初始化一个包：

```
npm init
```

当执行这个命令时，它会通过问答的形式来一步步进行设置。如果不需要修改默认的配置，直接一路回车即可。如果想跳过向导，快速生成一个package.json 文件，可以执行以下命令：

```
npm init --yes
```

其中，--yes可以简写为-y。这时生成的package.json文件的配置项就是 npm 的默认配置。当然这个默认配置也是可以更改的，可以通过类似下面这样的形式来修改 npm 的默认配置：

```
npm config set init.author.name YOUR_NAME  npm config set init.author.email YOUR_EMAIL  
```

当执行以上命令之后，之后再执行 npm init 命令时，package.json 的作者姓名和邮箱都会初始化为我们设定的值。

### 快速了解 package.json

当要使用一个包时，如果想要了解它是如何使用的，可以使用以下命令来打开这个包的主页，它会自动启动浏览器并打开这个页面，这里以React为例：

```
npm home react
```

如果想要查看这个包现存的issue，或者公开的roadmap，可以执行以下命令：

```
npm bugs react
```

如果想要查看这个包的代码地址，可以执行以下命令：

```
npm repo react
```

如果想要查看这个包的详细信息，可以执行以下命令：

```
npm info react
```

执行结果如下：

![1712468149082](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712468149082.png)

这里返回的是一个JavaScript对象，里面包含react模块的详细信息，可以通过info命令来获取这个对象的成员信息：

```
npm info react description
```

执行结果如下：

![1712468166473](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712468166473.png)

### 安装依赖

可以使用npm install命令来安装需要的包，如果想把这个包自动添加到package.json中，可以执行以下命令，这里以安装React为例：

```
npm install react --save
```

如果想要安装不同版本的包，可以这样：

```
// 安装最新版本
npm install react@latest
// 安装指定版本
npm install react@16.8.0
// 安装指定区间版本
npm install react@">=16.8.0 <17.0.1"
```

当使用npm安装依赖时，分为本地安装（local）和全局安装（global），它俩的区别就是是否包含-g参数：

| **命令**   | **简写** | **说明**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| 无         | 无       | 将模块安装到本地node_modules目录下，但不保存在package.json中。 |
| --save     | -S       | 将模块安装到本地node_modules目录下，同时保存到package.json中的dependencies配置项中，在生产环境下这个包的依赖依然存在。 |
| --sava-dev | -D       | 将模块安装到本地node_modules目录下，同时保存到package.json中的devDependencies配置项中，仅供开发时使用。 |
| --global   | -g       | 安装的模块为全局模块，如果命令行模块，会直接链接到环境变量中。 |

可以使用require关键字来引入本地安装的包。为了避免引用模块消失，保证依赖模块都会出现在package.json中，最好在npm install 时加上--save。

需要注意，在执行npm install命令时，npm 5 之前只会下载，不会保存依赖信息。如果需要保存，就需要加上 --save 选项， npm 5 以后就可以省略 --save 选项了，它会自动保存。

### 锁定依赖

当使用--save来安装依赖时，npm 会把这个依赖保存起来，并添加^前缀，他表示，当再次执行 npm install 命令时，会自动安装这个包在此大版本下的最新版本。如果想要修改这个功能，可以执行以下命令：

```
npm config set save-prefix='~'
```

执行完该命令之后，就会把^符号改为~符号。当再次安装新模块时，就从只允许小版本的升级变成了只允许补丁包的升级。

如果想要锁定当前的版本，可以执行以下命令：

```
npm config set save-exact true
```

这样每次 npm install xxx --save 时就会锁定依赖的版本号，相当于加了 --save-exact 参数。建议线上的应用都采用这种锁定版本号的方式。

为了彻底的锁定依赖的版本，让应用在任何机器上都安装同样的版本，可以执行以下命令：

```
npm shrinkwrap
```

执行这个命令之后，就会在项目的根目录产生一个npm-shrinkwrap.json配置文件，这里面包含了通过node_modules 计算出的模块的依赖树及版本。只要目录下有 npm-shrinkwrap.json 则运行 npm install 时就会优先使用 npm-shrinkwrap.json 中的配置进行安装，没有则使用 package.json 进行安装。

### 搜索依赖

npm 为我们提供了search 命令，用于搜索npm仓库，它搜索的参数可以是一个字符串，也可以是一个正则表达式：

```
npm search react
npm s react
```

搜索结果如下：

![1712468228245](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712468228245.png)

当然，我们也可以去node.js官网去找：https://www.npmjs.com/

想要找到一个合适的依赖包可能并不是一件容易的事。这时，可以使用网站https://npms.io/，这里将各个包的质量、受欢迎度、可维护性等指标做了量化。这些指标包括：是否使用了过时的依赖包、是否有代码检查配置、是否经过测试以及最近的版本是何时发布的等。

![1712468273008](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712468273008.png)

推荐使用 Openbase 搜索合适的包：https://openbase.com/

### 更新、卸载依赖

npm 为我们提供了更新依赖版本的命令：

```
npm update [package name]
```

如果想要更新全局安装的模块，需要添加参数 -global：

```
npm update -global [package name]
```

当执行这两个命令时，它会先到远程仓库查询最新版本，然后查询本地版本。如果本地版本不存在，或者远程版本较新，就会安装。

如果想要更新该依赖包在package.json中的版本，就需要使用-S或者--save参数。需要注意的是，从npm v2.6.1 开始，npm update只会更新顶层的模块，而不更新依赖的依赖模块，而之前的版本是递归更新的。如果想要这种效果，可以使用以下命令：

```
npm --depth 9999 update
```

除了可以更新包之外，还可以删除指定的包：

```
npm uninstall [package name]
```

如果想要删除全局的包，需要添加参数 -global：

```
npm uninstall [package name] -global
```

### 查找过时的包

npm 提供了一个命令来查看过时的依赖：

```
npm outdated
```

在项目中执行该命令，输出结果如下：

![1712468314536](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712468314536.png)

可以看到，这里列出了过时依赖的包名称、当前的版本、希望的版本、最新的版本、依赖在本地路径、依赖这个包的项目名称。

可以通过以下命令来检查npm包的最新版本：

```
// 展示包的信息
npm view <package-name>  
npm v <package-name>
// 展示最新版本
npm v <package-name> version
// 展示所有版本
npm v <package-name> versions
```

![1712468341083](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712468341083.png)

### 执行脚本

npm 不仅可以用于管理模块，还可以用于执行脚本。在package.json文件中有一个scripts字段，可以用于定义脚本命令，功npm 使用。我们除了可以在package.json文件中查看有哪些命令，也可以使用以下命令来查看所有脚本命令：

```
npm run
```

在项目中执行该命令之后的结果如下：

![1712468367982](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712468367982.png)

可以看到，这里定义了dev、build、build:test等命令，如果需要执行这些命令，只要这样执行即可：

```
npm run dev
npm run build
```

这里不在多说，这或许是我们平时用的最多的命令了，可以根据实际开发情况，来定制自己的npm命令。

### 安装可靠的依赖

可以使用 npm ci 命令来清理、安装依赖项。它通常用于CI/CD等自动化环境，使用它可以获得可靠的依赖。

```
npm ci
```

当执行该命令时，它会先删除本地的node_modules文件，因此它不需要去校验已下载文件版本与控制版本的关系，也不用校验是否存在最新版本的库，所以下载的速度相比npm install会更快。之后它会按照 package-lock.json 文件来安装确切版本的依赖项。并且不会将这个版本写入package.json或者package-lock.json文件。

使用该命令时，需要注意：

- 项目必需有 package-lock.json 或 npm-shrinkwrap.json 文件，如果没有，该命令将不起作用；
- npm ci 是 npm v6 中引入了的新命令，所以使用该命令时需要确保npm版本要>=5.7；
- npm ci 不能用来安装单个依赖，只能用来安装整个项目的依赖；
- npm ci 会安装 dependencies 和 devDependencies；
- 整个安装过程不会更新 package.json 或 package-lock.json 文件，整个安装过程是锁死的；
- 当package-lock.json中的依赖和package.json中不一致时，npm ci 会退出但不会修改package-lock.json文件。

### 删除重复的包

我们可以通过运行npm dedupe命令来删除重复的依赖项。该命令通过删除重复包并在多个依赖包之间共享公共依赖项来简化整体的结构。它会产生一个扁平的、去重的树。

```
npm dedupe
npm ddp
```

### 扫描漏洞

可以运行 npm audit 命令来扫描项目，来查找所有依赖项中存在的漏洞：

```
npm audit
```

下面是扫描结果：

![1712468445459](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712468445459.png)

可以运行以下命令来自动安装所有易受攻击包的补丁版本：

```
npm audit fix
```

### npm hooks

它是npm提供的一种机制，允许我们在特定的生命周期事件发生时触发自定义脚本。这些脚本可以用来自动化执行一些任务，如代码检查、构建应用程序等。

hooks近几年很火。熟悉的react-hooks、vue hooks、git hooks等

使用npm hooks非常简单，只需要在项目的package.json文件中的"scripts"字段下添加相应的脚本即可。例如，如果我们想要在npm install之前执行一个脚本，可以添加以下代码：

```js
"scripts": {  "preinstall": "node demo-script.js"}
```

当我执行npm install时候![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713687529944.png)会先执行我自定义js文件的日志输出

npm的hooks包括以下几种：

**1.生命周期hooks：**

在npm生命周期的各个阶段运行，包括preinstall、install、postinstall、preuninstall、uninstall、postuninstall、preversion、version、postversion等。

**2.包钩子：**

允许您配置URL端点，只要对任何受支持的实体类型发生更改，都将收到通知。钩子可以监视三种不同类型的实体：包、所有者和作用域。要创建一个包钩子，只需引用包名。要创建所有者挂钩，请在所有者名称前加上~（如~youruser）。要创建作用域挂钩，请在作用域名称前加上@（如@yourscope）。

**3.其他钩子：**

包括prepublishOnly、prepack、prepublish、preuninstall脚本等。

### npm view

npm view是一个命令行工具，用于查看远程npm仓库中的模块信息。它允许用户通过模块名或版本号来获取指定版本的模块信息，并将其打印到标准输出流中。

使用npm view命令时，可以指定要查看的模块名，并可选地指定要查看的版本范围。如果未指定版本范围，则默认将显示最新版本的模块信息

例如：

查看模块所有的版本信息：

```
npm view <module-name> versions
```

![1713687564360](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713687564360.png)查看模块的依赖关系信息：

```
npm view <module-name> dependencies
```

![1713687586479](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713687586479.png)查看模块的贡献者信息：

```
npm view <module-name> contributors
```

![1713687597049](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713687597049.png)查看模块的存储库URL：

```
npm view <module-name> repository.url
```

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713687607963.png)

### npm search

用于在npm库中搜索包。通过运行该命令，你可以在npm的官方库中搜索与你输入的关键字相关的包。相比npm view而言，这个属于模糊搜索包，npm view属于针对性看包信息。![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713687622976.png)

### npm root

用于查看当前包的安装路径。

例如，如果在很深的路径中，需要使用当前项目依赖的 node_modules/.bin 中的指令，可以使用以下命令找到目录：

```
npm root
```

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713687648781.png)

### npm prune

用于删除未使用的npm软件包。当你在项目中安装了大量的npm软件包时，可能会出现一些未使用的软件包，这些包会占用磁盘空间并可能导致项目运行缓慢。使用npm prune可以删除这些未使用的软件包，从而减少项目的大小并提高性能。

假如我之前安装了axios等等大量无用的包![1713687665123](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713687665123.png)但是我不想用命令一个一个的删，那就执行它![1713687680216](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713687680216.png)这样再去看node_modules里面就没有了。

### npm link

```
实习同学:大哥，帮忙发下依赖            
我:嗯，好。                 
10 分钟后            
实习同学：大哥，帮忙发下依赖               
我：嗯，好。                
20分钟后。            
实习同学：大哥，再帮忙发下依赖             
我：嗯，好。              
30分钟后。          
实习同学：大哥。滴滴。            
我：嗯？好。             
40分钟后。              
实习同学：大哥，再帮忙发下依赖                
我：嗯.....碰见什么问题了？你是一直被测试打回了吗?             
实习同学：没有，自测出来的问题。这个要重新发上去，然后重新装依赖。我才知道自己有没有改对。好麻烦啊。              
我:额.....em...是很麻烦。不过你的带教，没有告诉你怎么在本地改这玩意吗？你知道npm link 吗？            
实习同学：不知道。带教他很忙，这是干啥用的。
我：没事，npm link 一个非常不常用的命令。不知道很正常。我给你讲讲，(&**@#￥%……叭叭叭叭)
```

**我们的项目是如何寻找依赖文件的？**

在介绍npm link以前，我们先来了解一下，咱们的项目，是如何寻找依赖文件的。

*依赖文件系统会在node_modules文件夹下寻找。比如咱们项目依赖了一个antd。那么这个antd的依赖。会优先从当前目录下的node_modules下寻找，如果没有找到，则往父级目录寻找node_modules。一直到根目录为止。*

我们用（D:\文件夹1\文件夹2\项目文件夹）作为项目文件夹，画个图看看

![1717919119778](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1717919119778.png)

**npm link 把文件链接到哪里去了**

我们一般使用npm 下载全局依赖的时候，依赖会被默认安装到*C:\Users\你的用户名\AppData\Roaming\npm* 这个位置的node_modules中

![1717919147481](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1717919147481.png)

使用npm link也一样。npm link会在上述位置给你生产一个软连接（可以理解为快捷方式）。我们以gbye_util_pack这个测试包举个例子。

*npm unlink -g 包名* 则是删除这个包在全局的链接

注意：npm link是创建一个软连接，不是真的复制了一份代码到全局依赖里面。软链接文件前面的图标有一个小箭头。

![1717919166386](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1717919166386.png)

**如何使用被链接到全局的包？**

在需要引用全局包的地方，执行*npm link 包名* 命令即可

举个例子

gbyz_util_pack 这个包是我们刚才link到全局的包。然后我们在项目目录下执行 npm link gbyz_util_pack，

![1717919184726](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1717919184726.png)

然后这个包就会出现在当前项目文件夹的node_modules目录下。无需我们下载依赖。即可进行引用和调试。本地引用的包因为是软连接的形式，并不是克隆了一份代码，所以，被引用的包的更改，*会在引用项目中实时更新*。便于调试。

![1717919199941](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1717919199941.png)

**最后**

汇总下常用命令

npm link -会把当前文件夹的项目（包）链接到全局

npm unlink -g 包名 -取消当前文件夹项目在全局中的引用

npm link 包名 -引用被链接到全局的包到当前文件夹下的node_modules中

npm unlink 包名 -在当前文件夹项目中取消引用该全局包

##  查看已安装的包

可以通过以下命令来获取整个项目的包信息：

```
npm list
```

npm list 命令以树型结构列出当前项目安装的所有模块，以及它们依赖的模块。

![1712468479107](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712468479107.png)

如果加上global参数，就会列出全局安装的模块：

```
npm list -global
```

也可以查看指定包的依赖，比如在我现在做的项目下，执行以下命令：

```
npm list react
```

还可以使用npm ls 命令来查看指定包的依赖信息：

```
npm ls react
```

![1712468506056](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712468506056.png)

可以使用`--depth`参数来限制搜索的深度：

```
npm ls --depth=1
```

### 测试本地包

当我们在本地开发npm模块时，可以使用npm link命令来将本地的npm模块连接到对用的项目中去，便于对模块进行调试和测试。使用方式也很简单，在项目中执行以下命令：

```
npm link
```

执行完该命令之后，就会为这个npm包创建到全局，路径是` {prefix}/lib/node_modules/<package>`，它是一个快捷方式。之后我们就可以使用以下命令来在需要这个模块的项目中链接这个包：

```
npm link 模块名
```

这里的**模块名**就是依赖包的名称，也就是模块包的package.json文件中的name字段值。

如果不想继续使用了，执行以下命令来解除link即可：

```
npm unlink 模块名
```

## 遇到的问题

### 执行npm install命令时发生版本冲突问题

```js
// 执行npm install 命令发现报错：D:\StudySoft\VsCode\code\CODE_Projects\new-cms>npm install
npm ERR! code ERESOLVE
npm ERR! ERESOLVE unable to resolve dependency tree
npm ERR!
npm ERR! While resolving: panda@1.0.0
npm ERR! Found: react@17.0.2
npm ERR! node_modules/react
npm ERR!   react@"^17.0.2" from the root project
npm ERR!   peer react@">=16.9.0" from antd@4.24.8
npm ERR!   node_modules/antd
npm ERR!     antd@"^4.21.2" from the root project
npm ERR!     peer antd@"4.x" from @ant-design/pro-card@1.0.6
npm ERR!     node_modules/@ant-design/pro-card
npm ERR!       @ant-design/pro-card@"1.0.6" from the root project
npm ERR!   1 more (react-dom)
```

这个报错是因为依赖树出现了问题，可能是由于部分依赖的版本冲突导致的。

你可以尝试以下几种方法来解决这个问题：

1. 清空 node_modules 和 package-lock.json 文件，重新执行 npm install 命令。
2. 使用 npm install --legacy-peer-deps 命令替代 npm install 命令，这条命令会忽略 peerDependencies 的版本限制。
3. 更新 package.json 中的依赖版本号，使其符合 SemVer 规范。
4. 更换包管理器为 yarn 或 pnpm，并尝试再次执行安装命令。

如果以上方法不能解决问题，建议检查一下项目中 package.json 中的依赖是否正确，并且检查网络连接状态是否正常。

### 版本要求不兼容
```javascript
npm ERR! code ERESOLVE
npm ERR! ERESOLVE unable to resolve dependency tree
npm ERR! 
npm ERR! While resolving: vue-element-plus-admin@2.4.1
npm ERR! Found: vite@5.0.0
npm ERR! node_modules/vite
npm ERR!   dev vite@"5.0.0" from the root project
npm ERR! 
npm ERR! Could not resolve dependency:
npm ERR! peer vite@"^2.0.0 || ^3.0.0 || ^4.0.0" from vite-plugin-purge-icons@0.9.2
npm ERR! node_modules/vite-plugin-purge-icons
npm ERR!   dev vite-plugin-purge-icons@"^0.9.2" from the root project
npm ERR! 
npm ERR! Fix the upstream dependency conflict, or retry
npm ERR! this command with --force or --legacy-peer-deps
npm ERR! to accept an incorrect (and potentially broken) dependency resolution.
npm ERR! 
npm ERR! 
npm ERR! For a full report see:
npm ERR! /home/zhang/.npm/_logs/2023-11-23T08_05_13_256Z-eresolve-report.txt
 
npm ERR! A complete log of this run can be found in: /home/zhang/.npm/_logs/2023-11-23T08_05_13_256Z-debug-0.log
npm 错误！代码 ERESOLVE
npm 错误！ERESOLVE 无法解析依赖关系树
npm 错误！
npm 错误！解析时：vue-element-plus-admin@2.4.1
npm 错误！已找到： vite@5.0.0
npm 错误！node_modules/vite
npm 错误！  dev vite@“5.0.0” from the root project
npm 错误！
npm 错误！无法解析依赖项：
npm 错误！对等vite@“^2.0.0 ||^3.0.0 ||^4.0.0“ 来自 vite-plugin-purge-icons@0.9.2
npm 错误！node_modules/vite-plugin-purge-icons
npm 错误！  dev vite-plugin-purge-icons@“^0.9.2” 从根项目
npm 错误！
npm 错误！修复上游依赖冲突，或重试
npm 错误！此命令带有 --force 或 --legacy-peer-deps
npm 错误！接受不正确（且可能损坏）的依赖项解析。
npm 错误！
npm 错误！
npm 错误！有关完整报告，请参阅：
npm 错误！/home/zhang/.npm/_logs/2023-11-23T08_05_13_256Z-eresolve-report.txt
 
npm 错误！此运行的完整日志可在以下位置找到：/home/zhang/.npm/_logs/2023-11-23T08_05_13_256Z-debug-0.log

```
这个错误信息说明 npm 在尝试解析依赖关系时遇到了问题。具体来说，问题出现在 vite-plugin-purge-icons 这个插件上，它要求与 vite 的版本要匹配 ^2.0.0 || ^3.0.0 || ^4.0.0，这意味着它支持 vite 的 2.x、3.x、4.x 版本。但是你的项目中安装的 vite 版本是 5.0.0，这与 vite-plugin-purge-icons 的版本要求不兼容。

这里有几个解决方案：

1. **安装兼容的Vite版本**： 你可以尝试将 vite 降级到 4.x 或更低的版本以满足 vite-plugin-purge-icons 的要求。使用以下命令来安装兼容的版本：

```javascript
npm install vite@^4.0.0 --save-dev
```
或者，如果需要安装 3.x 版本的 vite：
```javascript
npm install vite@^3.0.0 --save-dev
```
2.**使用 --legacy-peer-deps**： 作为临时解决方案，你可以在安装时添加 --legacy-peer-deps 标志忽略依赖冲突：

```javascript
npm install --legacy-peer-deps
```
这会让 npm 忽略 peer 依赖版本冲突，但这可能会引入不兼容的依赖，因此使用时需要小心。

3.**使用 --force**： 另一个更激进的选项是使用 --force 标志强制安装，但这通常不推荐因为它可能导致更严重的问题：

```javascript
npm install --force
```
4.**检查是否有可用的插件更新**： 也许 vite-plugin-purge-icons 插件有一个更新的版本，它支持 vite 5.x。检查一下是否有更新的版本可用，并考虑升级：

```javascript
npm view vite-plugin-purge-icons versions npm install vite-plugin-purge-icons@latest --save-dev
```
5.**编辑 package.json 手动解决依赖问题**： 最后，你可以尝试手动编辑 package.json 文件来解决版本冲突，然后运行 npm install。

**选择哪种解决方案，取决于你对项目的依赖和兼容性要求。一般来说，尽量避免使用 --force 或 --legacy-peer-deps，因为这样可能会导致隐蔽的问题，在生产环境中尤其需要谨慎**。

### npm i 卡顿 处理方法
1.**原因一**：很多朋友直接使用npm外国源，因为国内访问国外网站的网络肯定慢，导致卡顿（这种最好解决）

解决方法：直接将npm源切换成淘宝源或者公司源即可
```javascript
npm config set registry https://registry.npm.taobao.org
```
查看镜像源地址
```javascript
 npm config get registry
```
2.**原因二**：npm（node）版本比较低

```javascript
// 查看npm版本
npm -v 
//  升级npm
npm install -g npm
```
3.**原因三**：公司使用的内网，连接不到镜像源上，需要切换到公司自己的镜像源上

```javascript
npm config set registry http://nexus.xx.cn/registry/xxxx/
// http://nexus.xx.cn/registry/xxxx/ 是公司自己的镜像源
npm config get registry
// 查看镜像源地址
```
4.**原因四**：删除node_modules重新npm install

5.**原因五**：使用yarn来进行安装

```javascript
// 全局安装yarn (mac需要加上sudo)
npm install -g yarn
 
// 成功后使用yarn install安装
yarn install
```
6.**原因六**：与本地npm相关资源，有冲突

解决方法：只要删除对应的文件夹，重新执行npm i 命令就可以

删除对应的文件
C:\Users\Administrator\AppData\Roaming\npm

7.**原因七**：需要本地与github配置ssh

   解决办法：根据提示，配置即可

> PS D:\xxxx\temp-cli> npm install inquirer
> npm ERR! code E401
> npm ERR! Unable to authenticate, need: BASIC realm="Sonatype Nexus Repository Manager"
> npm ERR! A complete log of this run can be found in:
> npm ERR!     C:\Users\Administrator\AppData\Roaming\npm-cache\_logs\2023-01-04T06_05_27_843Z-debug.log  
> PS D:\xxxx\temp-cli>

**问题原因：**
执行npm install到仓库拉取代码时需要认证，现在是认证不通过，因此报错 
**解决方法：**
先执行npm config list,找到.npmrc的位置

> PS D:\xxxx\temp-cli> npm config list
> ; cli configs
> metrics-registry = "http://192.168.xxx.xxx:8073/repository/npm-all/"
> scope = ""
> user-agent = "npm/6.14.12 node/v14.16.1 win32 x64"
> ; userconfig C:\Users\Administrator\.npmrc
> PS D:\xxxx\temp-cli>

如果 没有找到使用 npm config ls -l

> PS D:\xxxx\temp-cli> npm config ls -l
> ...
> ; "user" config from C:\Users\xxxx\.npmrc
> ....

![1713770586877](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713770586877.png)
**删除第二行，保存，重新npm i**

**网上其他人的办法：**

方法一：

>  删掉第二行 
>  //192.168.xx.xx:8073/repository/npm-all/:_authToken=NpmToken.1ee55b58-d164-3b43-a731-xxxxxx
>   重新执行登录
>  npm login --registry=http://192.168.xx.xxx:8073/repository/npm-internal/

  方法二：直接删掉.npmrc

> npm config set registry http://192.168.xx.xxxx:8073/repository/npm-all/ 
> npm config set //192.168.xx.xxx:8073/repository/npm-all/:_authToken=NpmToken.1ee55b58-d164-3b43-a731-xxxxx
> 说明：上面第一条命令是注册仓库的位置，第二条是仓库的的认证token
> 这里的token是从同事（他那里是正常）.npmrc文件中复制过来的
> 然后执行npm install正常了 

方法三

> 这里可能是当前的项目中没有登录公司的私有仓库，需要在该项目目录下重新登录公司的私有仓库
> group、hosted都需要重新登录
> npm config set registry='私有仓库地址'
> usernmae:
> password:
> email:
> 重新执行npm install 查看是否登录成功

### 无法加载文件 E:\NodeJS\npm.ps1

![1713772643886](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713772643886.png)

错误显示
npm : 无法加载文件 E:\NodeJS\npm.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 https:/go.microsoft.com/fwlin
 k/?LinkID=135170 中的 about_Execution_Policies。
 所在位置 行:1 字符: 1

npm init

  + CategoryInfo          : SecurityError: (:) []，PSSecurityException
  + FullyQualifiedErrorId : UnauthorizedAccess
复制
![1713772655383](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713772655383.png)
解决办法
第一步：执行如下命令查看权限

```javascript
get-ExecutionPolicy
```
![1713772665324](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713772665324.png)

显示禁用状态

第二步：执行如下命令设置可用

```javascript
set-executionpolicy remotesigned
```
![1713772677404](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713772677404.png)

测试成功
![1713772690881](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713772690881.png)