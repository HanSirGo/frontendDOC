# npm、yarn、pnpm 最佳实践

`npm`/`yarn` 等包管理工具是对包（`package`）的管理，`package` 指拥有 `package.json` 的一个文件夹（或tallbar），具体的内容可以**查看**[1]，其实就是已经打包好的代码。

## npm、yarn 命令

```
npm docs lodash === npm home lodash // 快速找到某个包的文档。
npm repo lodash // 快速找到某个包的 Github 仓库地址。

yarn upgrade rollup@last // 升级某一个包到最新版或者指定版本
yarn info @packagename // 查看包信息

yarn cache dir: /Users/juice/Library/Caches/Yarn/v6
yarn cache clean // 清除缓存
yarn config  set global-folder /Users/juice/Library/Caches/Yarn/v6
yarn global dir：/Users/juice/.config/yarn/global
yarn why <package>:来识别依赖冲突并解决它们。  

// 查看 yarn link

// 恢复yarn默认设置
rm ~/.yarnrc
rm -r .yarn
重新安装即可
npm install -g yarn
```

## npm 安装机制

它会优先安装依赖包到当前项目目录，使得不同应用项目的依赖各成体系，同时还减轻了包作者的 API 兼容性压力。

缺点：项目 A 和项目 B，都依赖了相同的公共库 C，那么公共库 C 一般都会在项目 A 和项目 B 中，各被安装一次。这就说明，**同一个依赖包可能在我们的电脑上进行多次安装。**

当然，对于一些工具模块比如 typescript 和 gulp「不过，一般还是建议不同项目维护自己局部的 gulp 开发工具以适配不同项目需求」，可以使用全局安装模式，这样方便注册 path 环境变量，这样就可以全局使用。

![1710054331047](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710054331047.png)image.png

1. npm install 执行之后，首先，检查并获取 npm 配置，**这里的优先级为：项目级的 .npmrc 文件 > 用户级的 .npmrc 文件> 全局级的 .npmrc 文件 > npm 内置的 .npmrc 文件**「~/.npmrc」。

2. 检查项目中是否有 package-lock.json 文件。

3. - 存在，则将缓存内容解压到 node_modules 中；
   - 否则就先从 npm 远程仓库下载包，校验包的完整性，并添加到缓存，同时解压到 node_modules。
   - 一致，直接使用 package-lock.json 中的信息，从缓存或网络资源中加载依赖；
   - 不一致，按照 npm 版本进行处理（不同 npm 版本处理会有不同，具体处理方式如图所示）。

4. 1. 如果有，则检查 package-lock.json 和 package.json 中声明的依赖是否一致：
   2. 如果没有，则根据 package.json 递归构建依赖树。然后按照构建好的依赖树下载完整的依赖资源，在下载时就会检查是否存在相关资源缓存：

5. 最后生成 package-lock.json。

构建依赖树时，当前依赖项目不管其是直接依赖还是子依赖的依赖，都应该按照扁平化原则，优先将其放置在 node_modules 根目录（最新版本 npm 规范）。在这个过程中，遇到相同模块就判断已放置在依赖树中的模块版本是否符合新模块的版本范围，如果符合则跳过；不符合则在当前模块的 node_modules 下放置该模块（最新版本 npm 规范）。

## npm 缓存机制

缓存可以解决 node_modules 中的依赖嵌套依赖重复通过网络安装问题，可以在缓存文件中获取提供安装效率。

**对于一个依赖包的同一版本进行本地化缓存，是当代依赖包管理工具的一个常见设计**。使用时要先执行以下命令：得到配置缓存的根目录在 /Users/juice/.npm（ Mac OS 中，npm 默认的缓存位置） 当中。cd 进入 /Users/juice/.npm 中可以发现 `_cacache` 文件。事实上，在 npm v5 版本之后，缓存数据均放在根目录中的_cacache文件夹中。

```
npm config get cache  // /Users/juice/.npm
npm cache clean --force // 清除缓存 /Users/juice/.npm/_cacache 
```

![1710054368768](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710054368768.png)image.png

打开`_cacache`文件，看看 npm 缓存了哪些东西，一共有 3 个目录：

- content-v2：里面基本都是一些二进制文件。为了使这些二进制文件可读，我们把二进制文件的扩展名改为 .tgz，然后进行解压，得到的结果其实就是我们的 npm 包资源。
- index-v5：我们采用跟刚刚一样的操作就可以获得一些描述性的文件，事实上这些内容就是 content-v2 里文件的索引
- tmp

**这些缓存如何被储存并被利用的呢？**

当 npm install 执行时，通过pacote把相应的包下载并解压在对应的 node_modules 下面。

检查是否有缓存

- 没有缓存，npm 下载依赖是通过 pacote 依赖 npm-registry-fetch 来下载包，先下载到缓存当中，pacote 再解压到项目 node_modules 下。npm-registry-fetch 可以通过设置 cache 属性，在给定的路径下根据IETF RFC 7234「定义了指定一个缓存新鲜度规则算法」生成缓存数据。
- 有缓存，根据 package-lock.json 中存储的 integrity、version、name 信息生成一个唯一的 key，这个 key 能够对应到 index-v5 目录下的缓存记录。如果发现有缓存资源，就会找到 tar 包的 hash，根据 hash 再去找缓存的 tar 包，并再次通过pacote把对应的二进制文件解压到相应的项目 node_modules 下面，省去了网络下载资源的开销。

注意，这里提到的缓存策略是从 npm v5 版本开始的。在 npm v5 版本之前，每个缓存的模块在 ~/.npm 文件夹中以模块名的形式直接存储，储存结构是：{cache}/{name}/{version}。

## npm link

如何高效率在本地调试以验证包的可用性呢？

- 一个“笨”方法是，手动复制粘贴组件并打包产出到业务项目的 node_modules 中进行验证，但是这种做法既不安全也会使得项目混乱，变得难以维护，同时过于依赖手工执行，这种操作非常原始。

- 使用 npm link。它本质就是软链接，可以将模块链接到对应的业务项目中运行，主要做了两件事：

- - 为目标 npm 模块（npm-package-1）创建软链接，将其链接到全局 node 模块安装路径 /usr/local/lib/node_modules/ 中；

- 为目标 npm 模块（npm-package-1）的可执行 bin 文件创建软链接，将其链接到全局 node 命令安装路径 /usr/local/bin/ 中。

我们来看一个具体场景，假设你正在开发项目 project 1，其中有个包 package 1，对应 npm 模块包名称是 npm-package-1，我们在 package 1 项目中加入了新功能 feature A，现在要验证在 project 1 项目中能否正常使用 package 1 的 feature A，你应该怎么做？

我们先在 package 1 目录中，执行 **npm link**，这样 npm link 通过链接目录和可执行文件，实现 npm 包命令的全局可执行。

然后在 project 1 中创建链接，执行 **npm link npm-package-1** 命令时，它就会去 /usr/local/lib/node_modules/ 这个路径下寻找是否有这个包，如果有就建立软链接。

这样一来，我们就可以在 project 1 的 node_module 中会看到链接过来的模块包 npm-package-1，此时的 npm-package-1 就带有最新开发的 feature A，这样一来就可以在 project 1 中正常开发调试 npm-package-1。当然别忘了，调试结束后可以执行 **npm unlink** 以取消关联。

注意 yarn link 的软链接在 `~/.config/yarn/link`

## npx 的作用[2]

npx 执行「npm execute」npm包的二进制文件， npx 由 npm v5.2 版本引入，解决了 npm 的一些使用快速开发、调试，以及项目内使用全局模块的痛点。

**在传统 npm 模式下**，如果我们需要使用代码检测工具 **ESLint**[3]，就要先通过 npm install 安装：

```
npm install eslint --save-dev

然后在项目根目录下执行：
./node_modules/.bin/eslint --init
./node_modules/.bin/eslint yourfile.js

而使用 npx 就简单多了，你只需要下面 2 个操作步骤：
npx eslint --init
npx eslint yourfile.js
```

为什么 npx 操作起来如此便捷呢？

这是因为它可以直接执行 node_modules/.bin 文件夹下的文件。在运行命令时，npx 可以自动去 node_modules/.bin 路径和环境变量 $PATH 里面检查命令是否存在，而不需要再在 package.json 中定义相关的 script。

npx 另一个更实用的好处是：npx 执行模块时会优先安装依赖，但是在安装执行后便删除此依赖，这就避免了全局安装模块带来的问题。

运行如下命令后，npx 会将 create-react-app 下载到一个临时目录，使用以后再删除：

```
npx create-react-app cra-project
```

**dlx**

download and execute

```
yarn 需要版本 2 以上。
yarn dlx create-react-app my-app
```

## npm 多源镜像和企业级部署私服原理

npm 中的源（registry），其实就是一个查询服务。以 npmjs.org 为例，它的查询服务网址是 **registry.npmjs.org/**[4]。这个网址后面跟上模块名，就会得到一个 JSON 对象，里面是该模块所有版本的信息。比如，访问 **registry.npmjs.org/react**[5]，就会看到 react 模块所有版本的信息。

我们可以通过npm config set命令来设置安装源或者某个 scope 对应的安装源，很多企业也会搭建自己的 npm 源。我们常常会碰到需要使用多个安装源的项目，这时就可以通过 npm-preinstall 的钩子，通过 npm 脚本，在安装公共依赖前自动进行源切换：

```
"scripts": {
    "preinstall": "node ./bin/preinstall.js"
}


其中 preinstall.js 脚本内容，具体逻辑为通过 node.js 执行npm config set命令，代码如下：

require(' child_process').exec('npm config get registry', function(error, stdout, stderr) {
  if (!stdout.toString().match(/registry.x.com/)) {
    exec('npm config set @xscope:registry https://xxx.com/npm/')
  }
})
```

国内很多开发者使用的 nrm（npm registry manager）是 npm 的镜像源管理工具，使用它可以快速地在 npm 源间切换，这当然也是一种选择。

**企业级部署私服的原因**

虽然 npm 并没有被屏蔽，但是下载第三方依赖包的速度依然较缓慢，这严重影响 CI/CD 流程或本地开发效率。部署镜像后，一般可以确保高速、稳定的 npm 服务，而且使发布私有模块更加安全。除此之外，审核机制也可以保障私服上的 npm 模块质量和安全。

**如何部署一个私有 npm 镜像呢？**

现在社区上主要有 3 种工具来搭建 npm 私服：nexus、verdaccio 以及 cnpm。

## npm 配置作用优先级

npm 可以通过默认配置帮助我们预设好 npm 对项目的影响动作，但是 npm 的配置优先级需要开发者确认了解。

如下图所示，优先级从左到右依次降低。我们在使用 npm 时需要了解 npm 的设置作用域，排除干扰范围，以免一顿骚操作之后，并没有找到相应的起作用配置。

![1710054443854](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710054443854.png)

## npm 镜像和安装问题

关于 npm 镜像和依赖安装问题，归根到底还是网络环境导致的。

如果没有条件，也不要紧，办法总比困难多，可以通过设置安装源镜像来解决，这就需要紧跟社区方案，刨根究底了。这里推荐一篇文章：**聊聊 npm 镜像那些险象环生的坑**[6]，文章中有更详细的内容，你可以看看。

**终端代理**

当安装包老是出现重试的情况可以考虑使用终端代理，开了全局代理以后，去 `网络偏好设置` 里。找到 `高级` 点进去切换到 `代理` 能看到。

```
export http_proxy=http://127.0.0.1:4780
export https_proxy=http://127.0.0.1:4780
```

![1710054494457](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710054494457.png)

## 最佳实操建议

1. 优先使用 npm v5.4.2 以上的 npm 版本，以保证 npm 的最基本先进性和稳定性。

2. 项目的第一次搭建使用 npm install 安装依赖包，并提交 package.json、package-lock.json，而不提交 node_modules 目录。

3. 对于升级依赖包的需求：

4. 1. 依靠 npm update 命令升级到新的小版本；
   2. 依靠 npm install @ 升级大版本；
   3. 也可以手动修改 package.json 中版本号，并执行 npm install 来升级版本；

5. 本地验证升级后新版本无问题，提交新的 package.json、package-lock.json 文件。

6. 对于降级依赖包的需求：执行 npm install @ 命令，验证没问题后，提交新的 package.json、package-lock.json 文件。

7. 删除某些依赖：

8. 1. 执行 npm uninstall 命令，验证没问题后，提交新的 package.json、package-lock.json 文件；
   2. 或者手动操作 package.json，删除依赖，执行 npm install 命令，验证没问题后，提交新的 package.json、package-lock.json 文件。

9. 任何团队成员提交 package.json、package-lock.json 更新后，其他成员应该拉取代码后，执行 npm install 更新依赖。

10. 任何时候都不要修改 package-lock.json。

11. 如果 package-lock.json 出现冲突或问题，建议将本地的 package-lock.json 文件删除，引入远程的 package-lock.json 文件和 package.json，再执行 npm install 命令。

# yarn

> Yarn 是一个由 Facebook、Google、Exponent 和 Tilde 构建的新的 JavaScript 包管理器。它的出现是为了解决历史上 npm 的某些不足（比如 npm 对于依赖的完整性和一致性保障，以及 npm 安装速度过慢的问题等）， npm 目前经过版本迭代汲取了 Yarn 一些优势特点（比如一致性安装校验算法等）。

**注意：虽然 yarn 也有其对应的仓库 https://registry.yarnpkg.com/，但其实它是 npmjs.org 的镜像。**

当 npm 还处在 v3 时期时，一个叫作 Yarn 的包管理方案横空出世。2016 年，npm 还没有 package-lock.json 文件，安装速度很慢，稳定性也较差，而 Yarn 的理念很好地解决了以下问题。

确定性：通过 yarn.lock 等机制，保证了确定性。即不管安装顺序如何，相同的依赖关系在任何机器和环境下，都可以以相同的方式被安装。（在 npm v5 之前，没有 package-lock.json 机制，只有默认并不会使用的npm-shrinkwrap.json。）

采用模块扁平安装模式：将依赖包的不同版本，按照一定策略，归结为单个版本，以避免创建多个副本造成冗余（npm 目前也有相同的优化）。

网络性能更好：Yarn 采用了请求排队的理念，类似并发连接池，能够更好地利用网络资源；同时引入了更好的安装失败时的重试机制。

采用缓存机制，实现了离线模式（npm 目前也有类似实现）。

## npm 和 yarn 的区别

1. npm 不同版本的 lock 文件作用是不同的，最新版本是 package.json + package-lock.json 共同决定锁定包，这点和 yarn 一致。
2. yarn2 每次安装完依赖都会自动执行 dedupe 命令，npm 和 yarn1 需要手动执行

## Yarn 安装机制

检测（checking）→ 解析包（Resolving Packages） → 获取包（Fetching Packages）→ 链接包（Linking Packages）→ 构建包（Building Packages）

1. 检测包（checking）

   这一步主要是检测项目中是否存在一些 npm 相关文件，比如 package-lock.json 等。如果有，会提示用户注意：这些文件的存在可能会导致冲突。在这一步骤中，也会检查系统 OS、CPU 等信息。

2. 解析包（Resolving Packages）

   这一步会解析依赖树中每一个包的版本信息。

   首先获取当前项目中 package.json 定义的 dependencies、devDependencies、optionalDependencies 的内容，这属于首层依赖。

   接着采用遍历首层依赖的方式获取依赖包的版本信息，以及递归查找每个依赖下嵌套依赖的版本信息，并将解析过和正在解析的包用一个 Set 数据结构来存储，这样就能保证同一个版本范围内的包不会被重复解析。

   对于没有解析过的包 A，首次尝试从 yarn.lock 中获取到版本信息，并标记为已解析；

   如果在 yarn.lock 中没有找到包 A，则向 Registry 发起请求获取满足版本范围的已知最高版本的包信息，获取后将当前包标记为已解析。

   总之，在经过解析包这一步之后，我们就确定了所有依赖的具体版本信息以及下载地址存储在一个 Set 数据结构。

![1710054527943](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710054527943.png)3.获取包（Fetching Packages）

这一步我们首先需要检查缓存中是否存在当前的依赖包，同时将缓存中不存在的依赖包下载到缓存目录。说起来简单，但是还是有些问题值得思考。

比如：如何判断缓存中是否存在当前的依赖包？其实 Yarn 会根据 cacheFolder+slug+node_modules+pkg.name 生成一个 path，判断系统中是否存在该 path，如果存在证明已经有缓存，不用重新下载。这个 path 也就是依赖包缓存的具体路径。

对于没有命中缓存的包，Yarn 会维护一个 fetch 队列，按照规则进行网络请求。如果下载包地址是一个 file 协议，或者是相对路径，就说明其指向一个本地目录，此时调用 Fetch From Local 从离线缓存中获取包；否则调用 Fetch From External 获取包。最终获取结果使用 fs.createWriteStream 写入到缓存目录下。

完整路径：~/Library/Caches/Yarn/v6/npm-@ahooksjs-use-request-2.8.13-5ace53859feb6b4fe9ebcbf2e72982bb9b7db383-integrity/node_modules/@ahooksjs/use-request

cacheFolder：~/Library/Caches/Yarn/v6/

slug：**npm-@ahooksjs-use-request-2.8.13-5ace53859feb6b4fe9ebcbf2e72982bb9b7db383-integrity**[7]

node_modules：node_modules/@ahooksjs

pkg.name：use-request

![1710054544394](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710054544394.png)image.png

4.链接包（Linking Packages）

上一步是将依赖下载到缓存目录，这一步是将项目中的依赖复制到项目 node_modules 下，同时遵循扁平化原则。在复制依赖前，Yarn 会先解析 peerDependencies，如果找不到符合 peerDependencies 的包，则进行 warning 提示，并最终拷贝依赖到项目中。

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710054554518.png)

构建包（Building Packages）

如果依赖包中存在二进制包需要进行编译，会在这一步进行。

## 依赖地狱

在 npm v2，是根据每个模块的依赖安装到模块下面的 node_modules ,依赖下面又有依赖，依此递归执行，最终形成了一颗巨大的依赖模块树。

缺点：

- 项目依赖树的层级非常深，不利于调试和排查问题；
- 依赖树的不同分支里，可能存在同样版本的相同依赖。比如直接依赖 A 和 B，但 A 和 B 都依赖相同版本的模块 C，那么 C 会重复出现在 A 和 B 依赖的 node_modules 中。
- 这种重复问题使得**安装结果浪费了较大的空间资源，也使得安装过程过慢，甚至会因为目录层级太深导致文件路径太长，最终在 Windows 系统下删除 node_modules 文件夹出现失败情况**。

## npm 包扁平化

npm v3 之后，node_modules 的结构改成了扁平结构

比如，项目 App 中，`A v1.0` 、`E v1.0` 模块依赖 `B v1.0` ，`C v1.0`、`D v1.0` 模块都依赖于 `B v2.0` ，

最终 `B v1.0` 会扁平化放到 node_modules下面，`B v2.0` 是放在 `C v1.0`、`D v1.0` 各自的node_modules下面。

注意：这在一定的层度上面缓解了依赖地狱的问题，为什么`B v1.0`、`B v2.0` 的安装方式仍然是依赖嵌套，这是一个历史的坑，如果全部扁平化，使用的时候到底应用 `B v1.0`、`B v2.0` 呢？

![1710054583279](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710054583279.png)image.png

当项目中的 `B v1.0` 全部升级到 `B v2.0` ，这时候 npm 的 `B v1.0` 依赖包依然存在 node_modules 中 ，并且会在首层 node_modules 中 下载 `B v2.0`，这样就会存在多余的文件，yarn 会自动执行 dedupe 命令。

- 删除 node_modules，重新安装。
- 通过命令：npm dedupe

![1710054597596](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710054597596.png)image.png

## yarn.lock 、package-lock.json 互转

使用工具 **github.com/imsnif/synp**[8]

## 对 yarn.lock 文件去重

**www.npmjs.com/package/yar…**[9]

它支持 yarn1 yarn2，但是如果包可能存在版本兼容问题，不建议使用 npm dedupe 和 yarn dedupe，因为可能会报错。

# pnpm[10]

pnpm 全局安装包「~/Library/pnpm/store/v3」，通过硬链接链接到项目上面使用。

## 前置知识

### 硬链接、软链接

通过 `ln -s` 创建一个软链接，通过 `ln` 可以创建一个硬链接。

```
ln -s package.json sort-package.json // 软链接
ln package.json test-package.json // 硬链接


软链接可理解为指向源文件的指针，它是单独的一个文件，仅仅只有几个字节，它拥有独立的 inode
硬链接与源文件同时指向一个物理地址，它与源文件共享存储数据，它俩拥有相同的 inode

-rw-rw-r--@  1 juice  staff   5.1K  5 30 19:46 README.md
-rw-rw-r--@  2 juice  staff   1.3K  5 30 19:46 package.json
drwxrwxr-x@  8 juice  staff   256B  5 30 19:46 public
lrwxr-xr-x   1 juice  staff    12B  8  9 22:26 sort-package.json -> package.json
drwxrwxr-x@ 14 juice  staff   448B  5 30 19:46 src
-rw-rw-r--@  2 juice  staff   1.3K  5 30 19:46 test-package.json
-rw-rw-r--@  1 juice  staff   535B  5 30 19:46 tsconfig.json
-rw-rw-r--@  1 juice  staff   402K  5 30 19:46 yarn.lock
```

使用硬链接的原因可能是：

- 硬链接指向的是物理地址，所以文件移动对 pnpm 没有影响，软链接则不行。
- 稳定性和跨平台兼容性，软链接在某些 windows 版本可能不太稳定的

## pnpm安装机制

pnpm 分三个阶段执行安装：是一个并发的过程。

1. 依赖解析， 仓库中没有的依赖都被识别并获取到仓库。

2. 1. 没有缓存，下载到这个仓库地址 ~/Library/pnpm/store/v3
   2. 有，下一步

3. 目录结构计算， `node_modules` 目录结构是根据依赖计算出来的。

4. 1. 根据依赖处理 .pnpm「虚拟存储目录」，处理硬链接、软链接问题

5. 链接依赖项。所有以前安装过的依赖项都会直接从 store 「仓库」中获取并链接到 `node_modules`。

6. 1. 为了满足 node 的解析规则，需要在 `node_modules` 下生成软连接依赖，这个依赖是根据第二步的硬链接去生成的软链接

![1710054634444](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710054634444.png)image.png

## pnpm 扁平化

参考：**pnpm.io/zh/symlinke…**[11]

**pnpm.io/zh/blog/202…**[12]

pnpm 为了实现 0 重复安装，巧妙运用了软硬链接的处理，也导致了难以理解的 node_modules 的目录结构。

```
pnpm add express axios
├── node_modules
    └── .pnpm // 虚拟存储目录，记录着所有的依赖，第一层为硬链接，硬链接下面又会有软链接和硬链接
    └── express // 这express、或其他 package 为软链接，用于满足模块的解析规则，它指向的是.pnpm/express@4.18.2/node_modules/express
    ... // 这个层级全是首层依赖的，比如这里就会有 axios
    └── .modules.yaml // 记录非首层依赖提升的依赖，在这里是除了 express
    
虚拟存储目录：存放着硬链接文件，软链接文件，命名规范为 .pnpm/<name>@<version>/node_modules/<name>
├── node_modules
    └── .pnpm
          ... // 这个层级全是硬链接
          └── body-parser@1.20.1 // 硬链接
                └── body-parser // 硬链接 指向 <store>/body-parser@1.20.1
                └── express // 软链接 指向 .pnpm/express@4.18.2/node_modules/express
          └── express@4.18.2 // 硬链接
                ... // 这个层级除了当前 packagename 文件 其他都为软链接
                └── body-parser // 软链接，指向 .pnpm/body-parser@1.20.1/node_modules/body-parser
                └── express // 硬链接 指向 <store>/express@4.18.2
                
通过这样的设计就可以解决所有的包都放在全局，并且可以在模块解析的时候都可以引用上。 

因为首层软链是根据首层依赖生成的，所以只有真正在依赖项中的包才能访问。
```

## pnpm 缺点

- **pnpm 中的 bin 是不存在的**[13]，所以只能找到真实的路径再去执行。这个问题已经解决了。
- preinstall 这个钩子目前是不能使用的，只能等到 install 结束后。

## pnpm 优点

- 解决幽灵依赖，pnpm 只会在 dependencies 相关字段下面的包，才会在 node_modules 中生成软链接，其他的依赖包放在 .pnpm中，根据模块解析规则，pnpm 这种设计自带解决幽灵依赖问题。
- 解决重复下载、重复打包的问题，相同版本的包如果在 store 中存在，则通过链接的形式引用，解决重复打包是相同版本都存在在一个位置「.pnpm文件夹里面」，webpack 等打包工具知道这个库路径是被引用了多少次，就会抽离出来「可配置 splitchunks」，避免重复打包，如果在 yarn 和 npm 它们的库路径是可能不一样的，导致一个库被重复打包，从而导致包的体积变大。

## public-hoist-pattern 配置

在 `pnpm` 中可通过 **public-hoist-pattern**[14] 配置，来决定忽略某些特定依赖的幽灵依赖。而在 pnpm 中的默认配置是 `['*eslint*', '*prettier*']`，它将会自动取消 `eslint`/`prettier` 的幽灵依赖。它可以解决就是工具链有些依赖是给插件使用的，但是由于pnpm 解决幽灵依赖让例如 eslint 等包失效，pnpm 把 *eslint* ,*prettier* 放在首层依赖，这样就可以避免工具链出错。

shamefully-hoist=true 同样可以解决这个问题，但是它是全部依赖都放在首层依赖，不推荐

![1710054671603](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710054671603.png)image.png

## npm、yarn迁移至 pnpm

```
npm install -g pnpm
.npmrc 文件需要配置一下相应的规则，一般是哪些依赖需要放在 node_modules/xx 下面，提升依赖  https://pnpm.io/zh/npmrc

// 将 npm、yarn 的lock 文件都转为 pnpm-lock.yaml
pnpm import
// 安装前拦截一下。
"scripts": {
  "preinstall": "npx only-allow pnpm", 
  ...
}
// 更新 cicd 命令
// 删除 node_modules,重新安装
```

## 命令

```
pnpm store path 查看缓存 ~/Library/pnpm/store/v3
```

# 问题

## 发包

```
注册 npm 账号
npm login
npm publish 即可


1.发布npm包的时候登录失败，可能是设置了其他镜像源

设置回原本的源，用来发布npm包
npm config set registry https://registry.npmjs.org
重新设置为淘宝镜像
npm config set registry https://registry.npm.taobao.org
查看npm当前设置的源
npm config get registry 或者 npm config list

2.NPM Publish Registry - 403 Forbidden - "You don't have permission to publish
包重名了
```

## 版本号计算

在一些手写八股文面试题中，会有一些题目与版本有关，比如：

计算两个版本号的大小 发版时，给固定版本号递增 检测版本号是否有效等等 在 npm 发包时，npm 会调用 **semver**[15] 这个包完成这些事情：

semver.valid('1.2.3') // '1.2.3'

semver.lt('1.2.3', '9.8.7') // true

semver.satisfies('1.2.3', '1.x || >=2.5.0 || 5.0.0 - 7.2.3') // true

## 模块查找规则[16]

对于一下核心模块，需要注意，当存在模块缓存的时候，**可以加一个 `node:`前缀**[17]。

![1710054733327](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710054733327.png)

## packageManger

### corepack

`npm` 是 `node.js` 的官方包管理工具，集成在 `node.js` 环境中，当你安装了 `node.js`，也自动安装了 `npm`。

而在最近的 `node.js` 版本中，也将 `yarn`/`pnpm` 集成在了 node.js 的集成工具 **corepack**[18] 上，通过 `corepack` 你可以无需安装自由选择你的包管理工具。

```
npm uninstall -g yarn pnpm
npm install -g corepack

corepack enable to install the required Yarn and pnpm binaries on your path.
corepack enable

查看版本
corepack pnpm --version

查找 pnpm 的路径，如果以前已安装，可能有多个路径
where pnpm
```

在 `package.json` 中，可通过 `packageManger` 字段定义项目中的包管理工具，以及版本号。

```
{
   "packageManager": "yarn@3.2.3+sha224.953c8233f7a92884eee2de69a1b92d1f2ec1655e66d08071ba9a02fa",
   "packageManager": "pnpm@8.6.10"
}

yarn 是软件包管理器的名称，指定版本为 3.2.3，并附带该版本的 SHA-224 哈希值用于验证。packageManager@x.y.z 是必需的，而哈希值是可选的，但强烈推荐作为安全实践。允许的软件包管理器取值为 yarn、npm 和 pnpm。

corepack 它将会自动安装 package.json 中指定的包管理工具以及版本号,如何在不合法的包管理器它会提醒，合法的即会采用 pa
```



**原文链接：https://juejin.cn/post/7302240874507042825**

