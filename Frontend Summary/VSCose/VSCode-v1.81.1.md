> VSCode版本：1.81.1

### 功能已经被 VSCode 内置

以下，列出了一些功能已经被VSCode内置的，以及提供这些功能的扩展的列表。卸载这些现在不再需要的扩展将提高你的编辑器性能和效率。

#### Auto Close Tag

![1713598147374](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598147374.png)

> 已发布: 2016/6/28 14:06:44
>
> 上次更新时间: 2022/2/8 10:28:37

当你添加一个新的HTML标签时，这个扩展功能会自动添加相应的闭合标签。这是一个非常优秀的扩展，下载量也是千万级的，可以看到很多人对它的肯定。但现在vscode已经内置了它的主要功能，我们没必要再安装一遍，占用宝贵的内存资源。

> 注意：VSCode 不支持在  `.vue` 文件中原生的自动闭合标签功能。您可以通过安装 Vue Languages Features (Volar) 来启用此功能。

#### Auto Rename Tag

![1713598248420](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598248420.png)

> 发布时间: 2016-7-3
>
> 上次更新时间:2022-2-8

这个和上面是一个作者的，在修改html标签时，它能自动重命名。现在vscode也内置了，而且在新版本中`jsx`、`tsx`中也已经支持html标签重命名。

在`settings.json`文件中增加配置：

```js
// settings.json
"editor.linkedEditing": true
```

不需要安装 Auto Rename Tag， vscode内置的功能就能实现了。

#### Trailing Spaces

![1713598311001](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598311001.png)

> 发布时间: 2016-3-8
>
> 上次更新时间:2022-7-11

这个扩展的功能是：自动删除末尾的空白字符，确保一致的格式。下载量也超过150万+。

VSCode现在将该功能内置，可以在文件中自动删除末尾的空白字符。不需要命令或突出显示，它会在保存文件时自动修剪文件，使其成为一个后台操作，无需再费心考虑末尾的空白字符问题。

将这个配置内容添加到你的`settings.json`文件以启用自动修剪：

```js
{  
"files.trimTrailingWhitespace": true,  
}
```

有些情况下可能希望关闭这个设置，例如使用vscode写markdown文档时，因为根据CommonMark规范，你必须在行的末尾放置两个或更多空格才能在输出中创建硬行换行。你可以将以下内容添加到你的settings.json文件来关闭它：

```js
{  
"[markdown]": {  
"files.trimTrailingWhitespace": false  
}  
}
```

#### 路径自动补全

大家主要在用的扩展有这两个：

- Path IntelliSense
- Path Autocomplete

其实VSCode已经具备原生的**路径自动补全功能**。当你准备输入要导入的文件名（通常在输入""），会列出一个项目中的文件列表，从中选择一个将自动插入文件名，我觉得路径自动补全扩展可以考虑卸载了！

#### Settings Sync

这个扩展的功能真的非常强大，相信你也被不少人推荐过~

![1713598417872](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598417872.png)

`Settings Sync`可以同步你当前的 VSCode 配置环境，当你需要在其它的电脑工作时，您不用重头再来一遍。新机器登录一下就搞定了。再也不用折腾环境了。

大致原理：使用GitHub Gist来同步多台计算机上的设置，代码段，主题，文件图标，启动，键绑定，工作区和扩展。

打开可以看到，它明确告诉我们不再维护了， 让使用内置的**同步功能**。我们看一下VSCode中怎么配置：

![1713598459047](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598459047.png)

#### HTML Snippets

![1713598476711](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598476711.png)

早年间相信大家写 HTML 时都会安装它，简单回顾一下当年它作用：

```
 ul>li.slide*3>p.item$
```

通过上面的写法，直接就能快速帮我们生成代码片段：

```html
<ul>
    <li class="slide">
        <p class="item1"></p>
    </li>
    <li class="slide">
        <p class="item2"></p>
    </li>
    <li class="slide">
        <p class="item3"></p>
    </li>
</ul>
```

遥想当年，真的觉得泰裤辣，写代码嗖嗖的~

和`HTML Snippets` 功能相似的还有 `CSS Snippets`用于扩展CSS的缩写片段。

现在VSCode内置 Emmet 功能，提供了像这些扩展一样的HTML和CSS片段。官方文档VSCode Emmet指南中写了，在默认情况下，在html、haml、pug、slim、jsx、xml、xsl、css、scss、sass、less和stylus文件中都启用了Emmet。

#### Bracket pair colorization

![1713598559762](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598559762.png)

这个扩展作用是显示多个彩色括号，看一下效果：

使用前：

![1713598580208](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598580208.png)

使用后：

![1713598593569](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598593569.png)

各个结对的括号兄弟都有了不同的颜色。不管是Code Review或是改代码，都便利了许多呢！

但是目前vscode也内置了，所以也不用再下载了，默认是开启的。如果没有开启，点击设置，搜索Bracket Pair，并勾选上以下设置：

![1713598614429](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598614429.png)

#### Auto Import

![1713598629552](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598629552.png)

> 发布时间: 2016-7-14
>
> 上次更新时间:2021-4-15

下载量330万+,使用这个扩展的用户也是非常的多。但也可以看到作者很久不更新了。

具有自动导入功能时，当文件中引用了模块的函数、变量或其他成员时，该模块会自动导入到文件中，从而节省时间和精力。如果模块文件被移动，这个扩展将帮助自动更新它们。现这些功能也被 VsCode 内置了。

VsCode 内置功能，设置自动导入：

- JavaScript > Suggest: Auto Imports: "启用/禁用自动导入建议"。默认情况下为 true。
- TypeScript > Suggest: Auto Imports: "启用/禁用自动导入建议"。默认情况下为 true。

![1713598663221](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598663221.png)

文件移动时更新设置：

- JavaScript > Update Imports on File Move: "启用/禁用在重命名或移动文件时自动更新导入路径的功能"。默认值为 prompt，表示会向您显示一个对话框，询问是否要更新移动文件的导入。将其设置为 always 将跳过对话框，而设置为 never 将完全关闭此功能。
- TypeScript > Update Imports on File Move: "启用/禁用在重命名或移动文件时自动更新导入路径的功能"。与前一个设置类似，它有可能的值是 prompt、always 和 never，默认值是 prompt。

![1713598681788](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598681788.png)

也可以直接在`setting.json`文件中设置这些属性来控制：

```js
{  
"javascript.suggest.autoImports": true,  
"typescript.suggest.autoImports": true,  
"javascript.updateImportsOnFileMove.enabled": "prompt",  
"typescript.updateImportsOnFileMove.enabled": "prompt"  
}
```

#### TypeScript Hero

![1713598717730](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598717730.png)

> 发布时间: 2016-8-05
>
> 上次更新时间:2021-4-13

这个百万级别下载量的扩展也可以放心的卸载了，VSCode 自身就是使用 TypeScript 编写的，`TypeScript` 相关的扩展的功能基本上全部已经被 VSCode 内置

以上都是由于VSCode 内置了功能，让原来的扩展已经不再需要额外安装了，下面再介绍几个已经不再维护的扩展，以及给大家推荐备选替换。

### 不再维护的扩展

#### Github

![1713598759325](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598759325.png)

> 发布时间: 2016-11-09
>
> 上次更新时间: 2021-10-22

Github扩展的作用是：在 VSCode 中直接审查和管理我们的GitHub拉取请求和问题。

这个扩展已经很久没有更新了，作者推荐使用 github 官方开发的 GitHub Pull Requests 作为替代品。之前安装的小伙伴该换就换吧

#### Beautify

![1713598781041](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713598781041.png)

这个老早以前就不维护了，因为 VSCode 内置的格式化器就是使用 `js-beautify`，但是前端当前最流行的格式化工具是 `prettier`，建议安装 `prettier`，然后设置 VSCode 使用 `prettier` 作为格式化器。

为什么要提这个，因为还是看到有人在混着用，然后出来说`prettier`这个配置失效了，那个设置不起作用。大家真的可以卸载了， 避免很多冲突。

