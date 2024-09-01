# 10个你应该立即卸载的VS Code扩展

![1724504499502](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504499502.png)

如果你发现VS Code随着时间变得越来越慢、越来越耗电，那么这个数字可能就是原因所在。

因为**每**一个新的扩展都会增加应用程序的内存和CPU使用量。

编程已经够具有挑战性了；没有人需要再与这样的事情抗争：

![1724504510183](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504510183.png)

所以我们需要尽量减少扩展的数量，以减少资源使用；同时还要防止这些扩展相互冲突或与原生功能冲突。

你知道，在市场中有相当多的扩展提供的功能，VS Code已经内置了。

通常，它们是在这些功能尚未添加时开发的；但一旦这些功能被添加，它们就成为了多余的附加项。

因此，在下面的列表中，我列出了一些VS Code已经集成的功能以及提供这些功能的扩展。卸载这些现在可有可无的扩展将提高你的编辑器的性能和效率。

我还将列出一些控制这些功能行为的设置。如果你不知道如何更改设置，这个指南将对你有帮助。

### **1. 自动关闭HTML标签**

当你添加一个新的HTML标签时，这个功能会自动添加相应的关闭标签。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/LDPLltmNy57TaaDEDia97vvQew8kpDj0YibUzfaicej1JtxgLkhqMQOF7s1RrqkzR38xCpO1afvEV7vgZxPu8QU6w/640?wx_fmt=gif&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1)

**扩展：**

- Auto Close Tag（1200万+下载）："自动添加HTML/XML关闭标签，与Visual Studio IDE或Sublime Text类似"。
- Close HTML/XML Tag（34.4万下载）："快速关闭最后打开的HTML/XML标签"。

**但是，功能已经内置：**

- HTML: Auto Closing Tags："启用/禁用HTML标签的自动关闭"。默认情况下为`true`。
- JavaScript: Auto Closing Tags："启用/禁用JSX标签的自动关闭"。默认情况下为`true`。
- TypeScript: Auto Closing Tags："启用/禁用JSX标签的自动关闭"。默认情况下为`true`。

![1724504543664](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504543664.png)

你可以将以下内容添加到你的`settings.json`文件中来启用它们。

![1724504554110](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504554110.png)

### **2. 路径自动补全**

路径自动补全功能在你导入模块或在HTML中链接资源时，提供一个项目中文件的列表供你选择。

**扩展：**

- Path IntelliSense（1250万+下载）："Visual Studio Code插件，自动补全文件名"。
- Path Autocomplete（170万+下载）："为Visual Studio Code和VS Code Web版提供路径补全"。

**但是，功能已经内置：**

VS Code已经原生支持路径自动补全。

当我输入文件名来导入时（通常在输入开始引号时），会显示一个建议的项目文件列表，供我快速选择。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/LDPLltmNy57TaaDEDia97vvQew8kpDj0Y9dU68YLwLwnMWDKwSeW5K2LTvyygmo1PZfC43GWnMK09OErwL1hgsg/640?wx_fmt=gif&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1)

### **3. HTML和CSS片段**

这些扩展通过添加你容易记住的缩写来帮助你快速插入常用的HTML和CSS片段。

**扩展：**

- HTML Snippets（1010万+下载）："完整的HTML标签，包括HTML5片段"。
- HTML Boilerplate（320万+下载）："基本的HTML5样板代码生成器"。
- CSS Snippets（22.5万+下载）："CSS简写片段"。

**但是，功能已经内置：**

Emmet 是VSCode内置的一个功能，它提供了类似于这些扩展的HTML和CSS片段。

如你所见，在VSCode Emmet官方指南中，它默认在 html、haml、pug、slim、jsx、xml、xsl、css、scss、sass、less 和 stylus 文件中启用。

简洁全面。

当你开始输入Emmet缩写时，一个建议将会弹出，并伴随自动补全选项；你还可以在VSCode的建议文档悬浮框中看到展开的预览。

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

### **4. 括号配对着色**

括号配对着色是一种流行的语法高亮功能，它根据括号的顺序为它们着色。

它使识别作用域变得更容易，并帮助你编写涉及许多括号的表达式，如单行函数组合。

![1724504578984](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504578984.png)

**扩展：**

- Bracket Pair Colorizer 2（610万+下载）："一个用于着色匹配括号的可自定义扩展"。它现在已经被弃用。
- Rainbow Brackets（190万+下载）："VS Code的彩虹括号扩展"。

**但是，功能已经内置：**

在看到对括号配对着色的需求以及将其作为扩展添加的性能问题后，VSCode团队决定将其集成到编辑器中。

在这篇博客中，他们表示，原生的括号配对着色功能比Bracket Pair Colorizer 2快一万倍以上。

以下是启用/禁用括号配对着色的设置：

`Editor > Bracket Pair Colorization`："控制是否启用括号配对着色"。默认情况下为`true`。

你可以通过在`settings.json`中添加以下内容来启用它：

settings.json

![1724504612930](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504612930.png)

可以使用的颜色最多有6种，适用于连续的嵌套级别。不过，每个主题都会有其最大颜色。例如，Dracula主题默认有6种颜色，但One Dark Pro主题只有3种。

![1724504649518](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504649518.png)

你可以使用`workbench.colorCustomizations`设置来自定义任何主题的括号颜色。

![1724504672229](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504672229.png)

我们在方括号（`[ ]`）中指定主题的名称，然后将值分配给相关属性。`editorBracketHighlight.foregroundN`属性设置第N组括号的颜色，最大值为6。

现在，这将是One Dark Pro的括号配对着色效果：

![1724504686881](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504686881.png)

### **5. 模块自动导入**

通过自动导入功能，当在文件中引用模块的函数、变量或其他成员时，模块会自动导入到文件中，节省了时间和精力。

![1724504700181](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504700181.png)

如果模块文件被移动，自动导入功能会自动更新它们。

![1724504729579](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504729579.png)

**扩展：**

- Auto Import（380万下载）："自动查找、解析并为所有可用的导入提供代码操作和代码补全。适用于TypeScript和TSX"。
- Move TS（81万下载）："用于移动TypeScript文件和文件夹，并更新工作区中的相对导入"。

**但是，功能已经内置：**你可以通过以下设置在VSCode中启用或禁用模块的自动导入。

- JavaScript > Suggest: Auto Imports："启用/禁用自动导入建议"。默认情况下为`true`。
- TypeScript > Suggest: Auto Imports："启用/禁用自动导入建议"。默认情况下为`true`。
- JavaScript > Update Imports on File Move："启用/禁用在VS Code中重命名或移动文件时自动更新导入路径"。默认值为`prompt`，这意味着会向你显示一个对话框，询问是否要更新移动文件的导入。将其设置为`always`会跳过对话框，而设置为`never`则完全关闭该功能。
- TypeScript > Update Imports on File Move："启用/禁用在VS Code中重命名或移动文件时自动更新导入路径"。与上一个设置类似，它的可能值为`prompt`、`always`和`never`，默认值为`prompt`。

![1724504750664](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504750664.png)

你可以使用`settings.json`中的这些属性来控制这些设置：



你还可以添加此设置，如果你希望在每次保存文件时组织导入。

![1724504774863](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504774863.png)

![1724504792624](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504792624.png)

这将删除未使用的导入语句，并将绝对路径的导入语句排列在顶部，提供了一种无需动手的方式来清理代码。

### **6. HTML标签自动重命名**

这是一个强大的功能，我在开始使用VS Code的几个月后才发现！

只需编辑起始标签，结束标签将自动更新以匹配：

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

![1724504807167](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504807167.png)

**扩展：**

- Auto Rename Tag（1770万下载）："自动重命名配对的HTML/XML标签，类似于Visual Studio IDE的功能"。

**但是，功能已经内置：**

我使用以下设置轻松实现标签自动重命名，而无需安装任何东西：

- Editor: Linked Editing："控制编辑器是否启用了链接编辑。根据语言的不同，相关符号如HTML标签在编辑时会被更新。" 默认情况下为`false`。

![1724504820430](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504820430.png)

### **7. 自动修剪尾随空格**

这个实用的功能会移除文件中所有行末的空白字符，以保持一致的格式。

**扩展：**

- Trailing Spaces（200万下载）："高亮显示尾随空格并快速删除它们！"。
- AutoTrim（3.54万下载）："尾随空白通常存在于编辑代码行、删除尾随单词等操作之后。此扩展跟踪光标所在的行号，当这些行不再有活动光标时，移除尾随的制表符和空格"。

**但是，功能已经内置：**

VSCode内置了一个设置，可以自动删除文件中的尾随空格。

它会在保存文件时自动修剪尾随空格，使其成为一个后台操作，你无需再考虑。

![1724504835137](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504835137.png)

这里是设置：

- Files: Trim Trailing Whitespace："启用时，将在保存文件时修剪尾随空白"。默认情况下为`false`。

![1724504846076](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504846076.png)

你可以将以下内容添加到`settings.json`文件中以启用自动修剪：

![1724504858072](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504858072.png)

你可能希望在Markdown文件中关闭此设置，因为根据CommonMark规范，你需要在行末放置两个或更多空格来创建一个硬换行。你可以在`settings.json`文件中添加以下内容来关闭它：

![1724504885112](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504885112.png)

或者，你也可以使用反斜杠（`\`）代替空格来创建硬换行。

### **8. HTML标签自动包装**

我无法数清有多少次我需要将一个HTML元素包裹在一个新的元素中——通常是一个div。

使用此功能，我可以立即将`<p>`标签包裹在一个`<div>`中，而不必费力地在上面插入一个`<div>`，在下面插入一个`</div>`。

![1724504896421](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504896421.png)

**扩展：**

- htmltagwrap（67.4万下载）："使用HTML标签包裹选中的代码"。
- html tag wrapper（45.8万下载）："通过按`ctrl+i`包裹选中的HTML标签，你也可以简单地更改包裹标签的名称"。

**但是，功能已经内置：**

借助内置的“使用缩写包裹”命令，我可以快速将标签包裹在任何标签类型中。

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

![1724504915542](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504915542.png)

你看到新包裹的名称如何根据你的输入发生变化了吗？

### **9. 多彩缩进**

缩进指南让你更容易追踪代码中的不同缩进级别。

**扩展：**

- Indent Rainbow（750万下载）："此扩展为文本前的缩进上色，在每一步使用四种不同的颜色交替"。

**但是，功能已经内置：**是的，VS Code已经将此功能作为内置功能。

我们只需将Editor > Guides: Bracket Pairs设置从“active”更改为“always”即可显示多彩缩进。

![1724504931347](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504931347.png)

从这样：

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

![1724504944660](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504944660.png)

到这样✅：

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

![1724504956082](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504956082.png)

漂亮。

### **10. NPM集成**

在每个严肃的项目中，你可能都有工具来自动化测试、代码检查、构建等任务。

因此，此功能使你可以通过点击一个按钮轻松启动这些任务。无需切换上下文。

**扩展：**

- NPM（680万下载）："此扩展支持运行在package.json文件中定义的npm脚本"。每次打开包含package.json的项目时，我都会看到它作为推荐扩展。

**但是，功能已经内置：**

借助内置的NPM脚本视图，我可以轻松查看项目`package.json`中的所有脚本，并运行我想要的任何脚本：

![1724504974127](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504974127.png)

唉，但现在你必须将鼠标拖到那里，只是为了运行一个简单的任务。

使用Tasks: Run Task命令要好得多：

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

![1724504993114](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724504993114.png)

“它还是太慢了！”

好吧，如果你知道确切的脚本名称，那就按Ctrl + `打开内置终端并满足你的CLI需求`：

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

![1724505010495](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724505010495.png)

### **最后总结**

这些扩展可能曾经在过去发挥了关键作用，但如今大部分已不再需要，因为VS Code已经将它们的功能内置。

卸载它们以减少臃肿，提高Visual Studio Code的效率。

