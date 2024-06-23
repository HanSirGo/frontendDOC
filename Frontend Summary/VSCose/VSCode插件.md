## VScode插件

#### Draw.io Integration

> ![1714804574206](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714804574206.png)
>
> 绘图软件
>
> 创建xx.dio或者xx.drawio 的文件开始绘图 drawio.png也可以

#### 驼峰翻译助手

![1713591953197](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713591953197.png)

中文一键翻译转换成常用大小驼峰、蛇形等格式。

![1713591983408](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713591983408.png)

安装好插件后，选择需要翻译的内容，按快捷键就可以调出上面的翻译内容，选择你需要的就行了。

```js
win: "Alt+shift+t" 
mac: "cmd+shift+t"
```

> 如果你选中的英文， 也可以快速的大小写、驼峰命名转换

#### GitHub Repositories

远程操作GitHub库

![1713592016270](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713592016270.png)

安装完插件后，在VS Code 左下角有个蓝色图标：

![1713592057469](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713592057469.png)

点击图标后，选择`打开远程存储库`：

![1713592073702](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713592073702.png)

例如我们想看 `ant-design` 的源码， 点击蓝色图标后，把 GitHub 上的源码地址复制进去就可以了。

![1713592100921](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713592100921.png)

可以看到`ant-design` 的源码瞬间就在vscode中了，还可以提交修改，岂不美哉！

![1713592115493](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713592115493.png)

VScode插件地址 [https://marketplace.visualstudio.com](https://marketplace.visualstudio.com/)
**我使用的VScode依赖包**
![1713700075283](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700075283.png)

#### 0. vscode-icons（已用）
> 修改文件图标
> 左下角 设置 ，点击 ‘主题 ’ ‘文件图标主题‘ ‘VSCode Icons’
> ![1713700096032](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700096032.png)

#### 1. Chinese (Simplified) (简体中文) Language Pack for Visual Studio Code（已用）
> 这是微软官方提供的 Visual Studio Code 语言包，专门为中文（简体）用户设计。
> https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-zh-hans
> ![1713700107046](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700107046.png)
#### 2. Git History（已用）
> 它允许您在 VSCode 中轻松查看文件的 Git 历史记录，帮助您理解代码的演变过程。
> https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory
> ![1713700116346](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700116346.png)
#### 3. GitLens —Git superCharged（已用）
> 包括文件注释、比较和提交历史 
> https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens
> ![](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700127573.png)
#### 4. Auto Rename Tag（已用）
> 当您重命名一个 HTML/XML 的开始或结束标签时，会自动重命名配对的 HTML/XML 标签。
> marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag
> ![1713700136744](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700136744.png)
#### 5. Auto Close Tag
> 输入开始标签的右括号后，将自动插入结束标签
> https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag![1713700145542](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700145542.png)
#### 6. Auto Complete Tag
> 拥有 Auto Close Tag 和 Auto Rename Tag 的功能
> https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-complete-tag
> ![1713700154190](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700154190.png)
#### 7. Vue Peek（已用）
> 可以让我们快速跳转到组件、模块定义的文件。
> https://marketplace.visualstudio.com/items?itemName=dariofuzinato.vue-peek![1713700164435](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700164435.png)
#### 8. IntelliCode
> 此扩展通过在完成列表顶部显示代码上下文的推荐完成项来提供 AI 辅助 IntelliSense。
> https://marketplace.visualstudio.com/items?itemName=VisualStudioExptTeam.vscodeintellicode
> ![1713700175492](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700175492.png)
#### 9. Live Server（已用）
> 通过插件可以开启本地服务
> https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer![1713700185670](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700185670.png)
#### 10. prettier （已用）
> 作用：格式化美化代码
> Ctrl + s 保存会自动格式化
> https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode
> ![1713700194735](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700194735.png)
#### 11. ESLint（已用）
> ESLint 的 npm 包 与 VSCode 的 ESLint 插件
> npm 包：是实际的 Lint 规则以及我们执行 Lint 的时候，控制代码如何去进行格式化的。
> VSCode 插件: 实际指向我们项目的 /node_modules/eslint 或者全局的 eslint，通过 ESLint 的规则，告诉 IDE，哪些地方需要飘红。也就是说插件是在解析我们的打开的文件，同时和规则对比，是否存在 ESLint 问题。 以及可以通过我们的 IDE 配置，在不同的时机去执行我们的 Lint，比如保存自动格式化。
> ESLint 支持以下几种格式的配置文件，如果同一个目录下有多个配置文件，ESLint 只会使用一个。优先级顺序如下：
> .eslintrc.js
> .eslintrc.yaml
> .eslintrc.yml
> .eslintrc.json
> .eslintrc
> package.json
> https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint
> ![1713700204320](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700204320.png)
#### 12. Bracket Pair Colorizer（vscode已内置）
> 着色器通过给每个大括号对附加不同的颜色，解决了代码中大括号对猎取的问题。
> 目前vscode已内置，点击设置，搜索  Bracket Pair，将“控制是否启用括号对着色。”勾选“√”。并且将“控制是否启用括号对指南。”选为“true”。
> https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer-2![1713700214747](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700214747.png)
#### 13. Peacock（已用）

> 通过颜色，Peacock 可以轻松识别工作空间。您所要做的就是打开一个 VS Code 工作区，按 **F1 打开命令面板，输入 Peacock，然后从编辑器的预定义颜色中进行选择。**
> https://marketplace.visualstudio.com/items?itemName=johnpapa.vscode-peacock
> ![1713700225623](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700225623.png)
#### 14. Colorize
> 可视化 CSS 颜色
> https://marketplace.visualstudio.com/items?itemName=kamikillerto.vscode-colorize
> ![1713700235831](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700235831.png)
#### 15. Code Spell Checker（已用）
> 代码拼写检查器是一个方便且广泛使用的拼写检查工具
> https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker
> ![1713700244799](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700244799.png)
#### 16. Turbo Console Log（√）
> 让您选择调试主题的变量并自动将日志消息添加到最近的相对行。这使您无需编写任何 console.log 代码即可快速调试。
> ctrl + alt + l （是L 不是 i） 选中变量之后，使用这个快捷键生成 console.log
> alt + shift + c 注释所有 console.log
> alt + shift + u 启用所有 console.log
> alt + shift + d 删除所有 console.log
> https://marketplace.visualstudio.com/items?itemName=ChakrounAnas.turbo-console-log
> ![1713700253726](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700253726.png)
#### 17. Regex Previewer
> 它以一个并排的窗口显示正则表达式的匹配情况，并根据你的正则表达式进行实时的更新。
> https://marketplace.visualstudio.com/items?itemName=chrmarti.regex
> ![](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700263411.png)
> ![1713700280880](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700280880.png)
#### 18. JavaScript (ES6) code snippets（已用）
> 除了支持JavaScript之外，JavaScript代码片段还支持TypeScript、TypeScript React、Html和Vue代码片段。所有的代码片段都带有最后的;分号，这里有一个非分号的分叉。
> JavaScript Code Snippets 包括类的助手，方法和控制台方法，以加快编码。
> https://marketplace.visualstudio.com/items?itemName=xabikos.JavaScriptSnippets
> ![1713700290702](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700290702.png)
#### 19. Path Intellisense（已用）
> Path Intellisense 提供了路径自动补全功能，可以帮助开发者更快地输入文件路径和文件名，减少输入错误和提高工作效率。
> https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense
> ![1713700299991](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700299991.png)
#### 20. koroFileHeader（已用）
> 用于生成文件的头部注释和函数注释的插件；头部注释： Ctrl+win+i  函数注释： Ctrl+win+t
> https://marketplace.visualstudio.com/items?itemName=OBKoro1.korofileheader![1713700309043](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700309043.png)
#### 20. TypeScript Vue Plugin (Volar)（已用）
> TypeScript编写vue文件，支持类型检查，提供代码提示
> https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin
> ![1713700317564](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700317564.png)
#### 21. Volar（已用）
> Vue 团队官方推荐 Volar 插件来代替 Vetur 插件，不仅支持 Vue3 语言高亮、语法检测，还支持 TypeScript 和基于 vue-tsc 的类型检查功能。
> https://marketplace.visualstudio.com/items?itemName=Vue.volar
> ![1713700325980](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700325980.png)
#### 22. Vue VSCode Snippets（已用）
> 为开发者提供最简单快速的生成 Vue 代码片段的方法，通过各种快捷键就可以在 .vue文件中快速生成各种代码片段。
> vbase会提示生成的模版内容
> 输入 vfor快速生成 v-for指令模版
> 输入 v3onmounted快速生成 onMounted生命周期函数
> https://marketplace.visualstudio.com/items?itemName=sdras.vue-vscode-snippets![](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700334051.png)
#### 23. Visual Studio Intellicode（已用）
> 这个插件旨在帮助开发人员提供智能的代码完成建议而构建的，并且已预先构建了对多种编程语言的支持。![1713700344093](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700344093.png)
#### 24. IntelliCode API Usage Examples（已用）
> 可让您查看其他开发人员如何使用给定函数的真实示例。显示的示例来自GitHub上的公共开源存储库。
> https://marketplace.visualstudio.com/items?itemName=VisualStudioExptTeam.intellicode-api-usage-examples
> ![1713700355903](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700355903.png)
#### 25. Image preview
> 在标签引入图片的时候，鼠标放上去可以进行图片的预览。
> https://marketplace.visualstudio.com/items?itemName=kisstkondoros.vscode-gutter-preview![1713700364599](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700364599.png)
> ![1713700374067](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700374067.png)
#### 26. SVG Viewer
> svg格式文件，可以进行预览的插件
> ![1713700383880](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700383880.png)![1713700392694](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700392694.png)
#### 27. Power Mode
> 这是在vscode写代码的时候，伴随着特效效果
> ![1713700407470](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700407470.png)
#### 28. Code Runner（√）
> 它允许您在 VSCode 中直接运行代码片段，提供了快速测试和调试的便捷方式。
> code runner运行js可以，[code runner运行ts乱码](https://codeleading.com/article/65702716210/)
> https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner
> ![在这里插入图片描述](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700417412.png)
> ![1713700429773](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700429773.png)
>
> ####  29.Template String Converter（√）

> 键入美元符号然后在字符串中打开花括号会将引号转换为反引号
> https://marketplace.visualstudio.com/items?itemName=meganrogge.template-string-converter
> ![1713700440100](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700440100.png)
```bash
 此插件安装后如果没有效果，需要在 settings.json 文件中配置需要支持的语言：
"template-string-converter.validLanguages": [  
>  "html",  
>  "vue",  
>  "svelte",   
>  "typescript",   
>  "javascript",   
>  "typescriptreact",  
>  "javascriptreact" 
>  ]
```
#### 30.Import Cost（√）
> Importcost可以在代码中显示导入的估计大小。
> https://marketplace.visualstudio.com/items?itemName=wix.vscode-import-cost
> ![1713700450645](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700450645.png)
> ![1713700458519](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700458519.png)
#### 31. TODO Tree（√）
> ，你看完代码加了下面这个注释 ：// TODO 以后会扩展这部分功能 当然，不知道这个「以后」是什么时候，一不小心以后变成遥遥无期，一部分原因是不想改，另一部分原因是写下这段注释的人时间久了就忘记了，这时候你需要「 TODO Tree 插件」，我们可以更方便的管理代码中的此类注释。
> 这个插件能帮你组织和管理TODO 注释，你在代码中注释的带 TODO 的标签会统一在侧边栏显示出来，当然不限于 TODO 注释，可以自定义管理标签比如 FIXME 等，可以基于标签过滤和筛选。
> 使用： // TODO xxxxx
> ![在这里插入图片描述](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700468383.png)
#### 32.Better Comments（√ 不怎么用）
> 比 TODO Highlight 更强大的功能吗？Better Comments 允许您根据不同类型的注释突出显示您的注释，从而将其提升到一个新的水平。
> 注意：好像只有ts js文件有用
> https://marketplace.visualstudio.com/items?itemName=aaron-bond.better-comments
> ![1713700477644](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700477644.png)![1713700487974](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700487974.png)
#### 33. Console Ninja

> 作为前端，调试是我们日常工作中最常用的一项工作了，通常的做法是：代码里添加console.log调试代码，然后到浏览器的devTool查看打印结果。也就是需要不断在编辑器和浏览器之间切换，有没有更好的方式呢？
>
> 答案显而易见， Console Ninja就是用来干这事得。
>
> ![1717919327898](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1717919327898.png)
>
> 使用也很简单，安装该插件后，代码运行后console.log后面就会显示其运行信息。
>
> ![1717919359447](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1717919359447.png)
>
> **那接口的怎么显示console.log(res)??**

#### .Tailwind CSS IntelliSense

> Tailwind CSS IntelliSense 提供了对 Tailwind CSS 的智能提示和自动补全功能，可以帮助开发者更快地编写 CSS 样式代码，减少输入错误和提高工作效率。
> ![1713700496279](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700496279.png)
#### .Quokka.js
> 它告诉你在编码时你是如何出错的。它很适合通过现场执行和结果来学习和测试。
> https://marketplace.visualstudio.com/items?itemName=WallabyJs.quokka-vscode![1713700505147](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700505147.png)
#### .TODO Highlight
> 突出显示代码注释
> https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight![1713700514002](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700514002.png)
> ![1713700524178](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700524178.png)
#### .carbon-now-sh
> 将代码生成一张图片
> 使用方法：
> 在 VSCode 中，选中需要生成图片的代码
> 打开命令托盘：Windows：Ctrl + Shift + P，Mac：Cmd + Shift + P
> 输入 Carbon，回车
> ![1713700532996](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700532996.png)
#### .Polacode
> 突出显示您想要的代码，它会自动创建一个格式化的代码文件。
> https://marketplace.visualstudio.com/items?itemName=pnp.polacode
> ![1713700541274](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700541274.png)
#### .open in brower

> 该插件可通过编辑器打开默认浏览器显示代码效果
> ![1713700547892](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700547892.png)
#### .CSS Peek

> 该扩展支持符号定义跟踪的所有正常功能，但它适用于 css 选择器（类、ID 和 HTML 标记）。
> Peek：内联加载 css 文件并在此处进行快速编辑。( Ctrl+Shift+F12)
> 转到：直接跳转到 css 文件或在新编辑器中打开它 ( F12)
> 悬停：悬停在符号 ( Ctrl+hover)上显示定义
> https://marketplace.visualstudio.com/items?itemName=pranaygp.vscode-css-peek
> ![在这里插入图片描述](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700555628.png)
#### .CSS Initial Value
> 显示每个 CSS 属性初始值的 VS Code 扩展。
> https://marketplace.visualstudio.com/items?itemName=dzhavat.css-initial-value![1713700563744](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700563744.png)
#### .Trailing Spaces

> 突出显示尾随空格并立即删除它们！
> https://marketplace.visualstudio.com/items?itemName=shardulm94.trailing-spaces![1713700573426](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700573426.png)
#### . Docker

> 它提供了 Docker 容器支持，使您能够更轻松地构建、运行和调试容器化应用程序。
> ![1713700580707](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700580707.png)
#### . Markdown All in One
> 如果您经常使用 Markdown 编写文档，它将成为您的好帮手，提供了丰富的 Markdown 编辑功能和预览功能。 
> ![1713700587935](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700587935.png)
#### . Partial Diff 
> 它能够高亮两个文件之间的区别，使您能够轻松比较和合并更改，提高协作效率。
> ![1713700594864](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700594864.png)
#### . Doxygen Documentation Generator

>它可帮助您快速生成代码注释和文档
>![1713700602579](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700602579.png)
#### . Material Icon Theme 

> 文件和文件夹添加漂亮的材质设计图标。
> https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme
> ![1713700609485](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700609485.png)
#### .JavaScript Debugger
> JavaScript 调试器
> https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly
> ![在这里插入图片描述](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700617446.png)
#### .Remote Development
> 可以再物理或虚拟机器上实现无缝远程容器开发
> ![1713700624565](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700624565.png)
#### . Autoprefixer
> 自动补全css兼容前缀
> ![1713700632883](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713700632883.png)

> 设置-搜索Autoprefixer 选择在settings.json中编辑
> 在autoprefixer.options里添加如下代码：
```javascript
    "autoprefixer.options": {
        "browsers": [
            "ie >= 6",
            "firefox >= 8",
            "chrome >= 24",
            "Opera >= 10",
            "last 2 versions",
            "> 5%"
        ]
    }
```
> 配置参数:
> "ie >= 6":IE浏览器版本大于等于6
> " >5%":全球超过5%人使用的浏览器
> “last 2 versions”: 所有浏览器兼容到最后两个版本

> 注意：在处理兼容IE浏览器时，要注意IE浏览器较低版本本身就不支持的属性，即使进行兼容处理也不能应用。

> 使用方法： 在需要处理的css文件里F1，选择Autoprefixer：Run选项