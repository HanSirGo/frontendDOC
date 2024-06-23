## nvm

### 下载包

```
nvm管理node，npm管理包，yarn也一样；
nvm是一个node的版本管理工具，可以简单操作node版本的切换、安装、查看。。。等等
地址： https://github.com/coreybutler/nvm-windows/releases
```

![1709187364358](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709187364358.png)

```js
和系统相关的最好安装在C盘
nvm命令
nvm命令行操作命令
1,nvm nvm list 是查找本电脑上所有的node版本
 
- nvm list 查看已经安装的版本
- nvm list installed 查看已经安装的版本
- nvm list available 查看网络可以安装的版本
 
2,nvm install 安装最新版本nvm
 
3,nvm use <version> ## 切换使用指定的版本node
 
4,nvm ls 列出所有版本
 
5,nvm current显示当前版本
 
6,nvm alias <name> <version> ## 给不同的版本号添加别名
 
7,nvm unalias <name> ## 删除已定义的别名
 
8,nvm reinstall-packages <version> ## 在当前版本node环境下，重新全局安装指定版本号的npm包
 
9,nvm on 打开nodejs控制
 
10,nvm off 关闭nodejs控制
 
11,nvm proxy 查看设置与代理
 
12,nvm node_mirror [url] 设置或者查看setting.txt中的node_mirror，如果不设置的默认是 https://nodejs.org/dist/
　 nvm npm_mirror [url] 设置或者查看setting.txt中的npm_mirror,如果不设置的话默认的是： https://github.com/npm/npm/archive/.
 
13,nvm uninstall <version> 卸载制定的版本
 
14,nvm use [version] [arch] 切换制定的node版本和位数
 
15,nvm root [path] 设置和查看root路径
 
16,nvm version 查看当前的版本
```

### 终端下载

```js
// 首先在终端中通过以下命令来安装 nvm：

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash

# 当我输入该命令后，终端就报错了，报错信息：curl: (7) Failed to connect to raw.githubusercontent.com port 443 after 5 ms: Couldn't connect to server，这个报错是因为github 有些域名访问不到，可以通过配置hosts里面的ip域名对应关系解决。
```

```js
// 在终端中输入以下命令来打开hosts文件以进行编辑：
sudo vim /etc/hosts

// 在文件中追加以下对应关系：
199.232.68.133 raw.githubusercontent.com
199.232.68.133 user-images.githubusercontent.com
199.232.68.133 avatars2.githubusercontent.com
199.232.68.133 avatars1.githubusercontent.com
```

![1711791691745](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711791691745.png)

```
然后按下ESC键，输入:w来保存即可。这个配置不仅可以解决 nvm 的安装问题，还能使加载不了图片的 GitHub页面恢复正常，homebrew 也能装了。然后再输入上面的命令就可以安装nvm了。

安装完成之后，重新打开终端，输入 nvm，如果出现以下内容就是安装成功了：
```

![1711791733217](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711791733217.png)

```js
// 如果报错：zsh: command not found: nvm，那就是安装失败了。原因就是电脑缺少 bash_profile 文件。该文件位于/Users/MacUserName/.bash_profile路径，如果没有就创建一个，通过以下命令来创建：

touch .bash_profile

// 然后通过以下命令来打开该文件：

open .bash_profile

// 在文件中添加以下内容：

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" 

// 保存之后，在终端中输入以下命令来刷新配置：

source ~/.bash_profile

// 重新打开终端，输入nvm 来查看：
```

![1711791792781](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711791792781.png)

> 安装成功就可以愉快的玩耍啦，nvm 提供了很多命令管理 Node.js 版本。当输入 nvm 时，也会列出来所有可用的命令，下面来看看常用的 nvm 命令。
>
> 1. **安装 Node.js 版本：**
>
> 2. - 安装最新稳定版的 Node.js：`nvm install stable`
>    - 安装指定版本的 Node.js：`nvm install <version>` （例如：`nvm install 14.17.0`）
>
> 3. **切换 Node.js 版本：**
>
> 4. - 切换到已安装的其中一个版本：`nvm use <version>` （例如：`nvm use 14.17.0`）
>    - 可以通过简写版本号进行切换（例如 `nvm use 14`），nvm 将自动选择符合的已安装版本。
>    - 如果在项目目录中创建了 `.nvmrc` 文件，nvm 在进入该目录时会自动切换到文件中指定的 Node.js 版本。
>
> 5. **查看已安装的 Node.js 版本：**
>
> 6. - 列出已安装的所有版本：`nvm ls`
>    - 列出远程可用的所有版本：`nvm ls-remote`
>
> 7. **卸载 Node.js 版本：**
>
> 8. - 卸载指定的 Node.js 版本：`nvm uninstall <version>` （例如：`nvm uninstall 14.17.0`）
>
> 9. **设置默认的 Node.js 版本：**
>
> 10. - 设置默认版本：`nvm alias default <version>` （例如：`nvm alias default 14.17.0`）
>     - 这样，在新打开的终端中将自动使用默认版本。
>
> 11. **运行 Node.js 命令和 npm：**
>
> 12. - 在已安装的 Node.js 版本下运行命令：`nvm exec <version> <command>` （例如：`nvm exec 14.17.0 node -v`）
>     - 在当前使用的 Node.js 版本下运行命令：`nvm run <command>` （例如：`nvm run node -v`）

## nvm-desktop

> nvm-desktop 是一个以可视化界面操作方式管理多个 Node 版本的桌面应用，使用 Electron 构建（支持 Macos 和 Windows 系统）。通过该应用，可以快速安装和使用不同版本的 Node。它完美支持为不同的项目单独设置和切换 Node 版本，不依赖操作系统的任何特定功能和 shell。
>
> nvm-desktop 的功能包括：
>
> - 支持为系统全局和项目单独设置Node引擎版本
> - 管理Node的命令行工具
> - 支持英文和简体中文
> - 支持自定义下载镜像地址 (默认是 https://nodejs.org/dist)
> - Windows 平台支持自动检查更新
> - 完整的自动化测试

![1711793329424](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711793329424.png)

![1711793341965](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711793341965.png)

### 下载

> 首先，在 nvm-desktop 的 Release 页面**[2]**下载系统对应的版本：
>
> ![1711793380678](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711793380678.png)

### 环境配置

```js
// 安装完成之后，如果使用的是 Mac 电脑，需要在~/.bashrc、 ~/.profile 或 ~/.zshrc 文件添加以下内容，以便在登录时自动获取它：

 
export NVMD_DIR="$HOME/.nvmd" 
export PATH="$NVMD_DIR/bin:$PATH"
```

> 如果电脑系统默认的是 zsh, 可以复制这个命令添加到 `~/.zshrc` 文件中即可。如果电脑使用的是 bash，则复制粘贴到 `~/.bashrc` 文件中去即可。如果有其他安装问题，可以查看官方文档：https://github.com/1111mp/nvm-desktop/blob/main/README-zh_CN.md

> Windows 下则不需要额外的操作，安装好运行之后直接搜索指定的 Node.js 版本点击下载安装即可。

![1711793493909](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711793493909.png)

![1711793511407](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711793511407.png)

> 可以在终端中查看是否切换成功：

![1711793557770](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711793557770.png)

> nvm-desktop 还支持为每个项目设置不同的 Node.js 版本，只需从本地添加项目，并设置需要的版本即可：

![1711793588559](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711793588559.png)

> 这样设置之后，全局的 Node.js 版本和项目的 Node.js 版本互不干扰。
>
> 除此之外，点击版本名称可以查看该版本的更新日志，点击右上角的“远程刷新”按钮可以获取最新的 Node.js 版本：

![1711793629915](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711793629915.png)

> 支持搜索 Node.js 版本、 V8 版本、NPM 版本，支持按发布时间排序，对不同版本进行筛选：

![1711793652450](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711793652450.png)

> 在 Mac 上，支持在顶部菜单栏便捷修改 Node.js 版本：

![1711793692236](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711793692236.png)

> 在 Windows 上，支持在右下角菜单便捷修改 Node.js 版本：

![1711793712857](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711793712857.png)

## Mise

### **Node.js 版本问题**

在前端开发的世界中，`Node.js` 版本管理是一个常见且棘手的问题。

许多开发者在`启动项目`时，可能会遇到因`版本不匹配`而导致的项目无法运行的情况。这不仅会拖慢开发进度，还可能在接手旧项目时带来额外的挑战。

想象一下，刚刚接手一个遗留项目，却发现本地的 `Node` 版本是 `V20`，而项目却需要 `V14` 才能正常运行。这不仅令人沮丧，还可能需要您花费宝贵的时间去下载和配置正确的 `Node` 版本。

但不必担忧，`Mise` 来帮您解决这个问题。

也许有同学会说：`nvm 不是也可以解决这个问题吗？`

确实，`nvm` 是一个广泛使用的 `Node.js` 版本管理工具，它允许用户在同一台机器上安装和使用`多个版本`的 `Node.js`。

对于那些熟悉 `nvm` 的开发者来说，它提供了一个简单的方式来解决版本兼容性问题。然而，`Mise` 带来了一些独特的优势和特点，这些可能会吸引那些寻求更全面解决方案的开发者

### **什么是 Mise**

`Mise` 是一个基于`Rust`实现的多语言工具版本管理器！

![1718453480119](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718453480119.png)

它以其卓越的灵活性和强大的功能，优雅地替代了 `asdf`、`nvm`、`pyenv`、`rbenv` 等传统工具，为开发者提供了一种全新的、统一的方式来管理`不同语言`的开发工具和版本。

作为一款卓越的多语言工具版本管理器，`Mise` 能够让您轻松管理不同项目的 Node.js 版本需求：

- **多语言支持**：与 nvm 仅管理 Node.js 版本不同，Mise 是一个多语言工具版本管理器，支持包括 Node.js 在内的多种编程语言和工具，如 Python、Ruby、Go 等。
- **无缝切换**：Mise 允许您在不同版本的 Node.js 之间无缝切换，确保每个项目都能使用到它所需的确切版本。
- **环境一致性**：无论您处理多少项目，Mise 都能保证开发环境的一致性，减少因版本差异引起的问题。
- **简化配置**：使用 Mise，您不再需要手动下载和配置 Node 版本，它会自动为您处理这一切。
- **提高效率**：Mise 的自动化功能节省了您的时间，让您能够专注于编码和创新，而不是环境配置。

### **支持的语言和特点**

- **Bun**：现代的 JavaScript 运行时，Mise 确保您可以轻松切换和使用不同版本的 Bun。
- **Deno**：安全的 JavaScript 和 TypeScript 运行时，Mise 让您能够无缝管理 Deno 的多个版本。
- **Erlang**：用于构建可扩展和可靠的系统的编程语言，Mise 提供了对 Erlang 版本的精细控制。
- **Go**：简洁、高效的编程语言，Mise 支持 Go 的多版本管理，让开发更加灵活。
- **Java**：广泛使用的跨平台语言，Mise 允许开发者根据不同项目需求选择合适版本的 Java。
- **Node.js**：流行的 JavaScript 运行环境，Mise 提供了对 Node.js 版本的全面管理。
- **Python**：强大的多用途编程语言，Mise 使得 Python 版本的管理变得简单直观。
- **Ruby**：优雅而富有表现力的语言，Mise 帮助您在不同项目中使用特定版本的 Ruby。
- **Rust**：注重性能和安全的系统编程语言，Mise 支持 Rust 的多版本管理，让系统开发更加高效。

### **以 Node.js 为例**

##### **安装 Mise**

第一步先下载`Mise Cli`：

```
curl https://mise.run | sh
```

查看版本：

```
~/.local/bin/mise --version
mise 2024.x.x
```

##### **安装指定版本 Node.js**

安装 `node V20` 版本，并使其成为`全局默认版本`：

```
mise use -g node@20
```

查看 Node.js 版本：

```
node -v
v20.0.0
```

##### **管理环境变量**

`Mise` 通过`.env`文件来管理环境变量，首先在项目目录下创建一个`.env`文件：

```
echo 'DATABASE_URL=postgres://user:pass@localhost/dbname' > .env
```

然后 Mise 会自动加载这个文件中的环境变量：

```
mise env
```

##### **任务管理**

Mise 提供一个简单的任务管理功能，项目目录下创建一个`mise.yml`文件：

```
# mise.yml
tasks:
  build:
    - npm install
    - npm run build
  test:
    - npm test
```

执行任务：

```
mise run build
```

### **Windows 支持问题**

Mise 目前`不支持 Windows` 操作系统。它支持的操作系统和架构包括：

- macOS（x64 和 arm64）
- Linux（x64、x64-musl、arm64、arm64-musl、armv6、armv6-musl、armv7、armv7-musl）

对于 Windows 用户，可能需要寻找其他工具或等待 Mise 未来可能的 Windows 支持更新。

并且在 `Github` 中有相关的讨论帖子：

> ```
> https://github.com/jdx/mise/discussions/66
> ```

主题是`关于 Windows 支持`的。在这个讨论中，有人分享了他们对 mise 项目的一些想法和尝试，包括使用 Lua 作为插件的主要语言，并围绕它创建一套标准化的 API，以便于插件的开发和使用。

讨论中提到了一些关键点：

- **Lua 插件**: 作者提出了使用 Lua 作为插件语言的想法，并考虑了向后兼容性，以便现有的 asdf 插件能够与 mise 兼容。
- **API 设计**: 讨论了一组 API 的概念，这些 API 将提供执行各种操作的功能，例如平台检测、子进程生成、HTTP 请求、文件下载和 JSON 数据处理等。
- **示例脚本**: 提供了一个 Lua 脚本示例，展示了如何使用这些 API 来实现插件的功能，例如列出、下载、安装和获取最新稳定版本的命令。

这个讨论表明 Mise 社区正在积极探索扩展其功能和兼容性，包括对 `Windows 系统`的支持。

虽然讨论帖本身并没有直接说明 mise 目前已经支持 Windows，但它揭示了社区对于跨平台支持的兴趣和开发动态。

### **Mise 相关参考**

Mise 的设计理念是提供一个简洁、一致的接口，让开发者能够专注于创新和构建，而不必被环境配置的复杂性所困扰。通过 Mise，您可以轻松地在不同项目之间切换，确保每个项目都能使用到正确的工具版本，从而提高开发效率和项目的可维护性。

- **官网**：`https://mise.jdx.dev/`
- **Github 地址**：`https://github.com/jdx/mise`
- **Windows 系统支持相关**：`https://github.com/jdx/mise/discussions/66`