# 2024 JavaScript 调试参考文档

使用断点暂停代码，设置断点，以便您可以在代码执行过程中暂停代码。

### 暂停时检查值

执行暂停时，调试程序会评估当前函数中的所有变量、常量和对象，直到达到某个断点。调试程序会在相应声明旁边显示内嵌的当前值。

![1725186745194](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725186745194.png)

您可以使用控制台查询已求值的变量、常量和对象。

![1725186757664](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725186757664.png)

> 要点：执行暂停时，您还可以重启当前函数，甚至对其进行实时修改。

### 悬停时预览类/函数属性

执行暂停时，将鼠标悬停在类或函数名称上可预览其属性。

![1725186770705](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725186770705.png)

### 单步调试代码

代码暂停后，请一次一个表达式地单步调试，并在此过程中调查控制流和属性值。

跳过代码行

当代码行暂停时，如果代码行包含与正在调试的问题无关的函数，请点击 Step over 单步跳过 以执行该函数而不进入其中。

![1725186782317](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725186782317.png)

例如，假设您正在调试以下代码：

```
function updateHeader() {  var day = new Date().getDay();  var name = getName(); // A  updateName(name); // D}function getName() {  var name = app.first + ' ' + app.last; // B  return name; // C}
```

你已暂停A。按 Step over，开发者工具会执行您正在单步执行的函数（B 和 C）中的所有代码。然后，开发者工具会在 D 暂停。

### 单步进入代码行

当代码行暂停时，如果代码行包含与您正在调试的问题相关的函数调用，请点击 Step into 图标 单步进入 以进一步调查该函数。

![1725186810369](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725186810369.png)

例如，假设您正在调试以下代码：

```
function updateHeader() {  var day = new Date().getDay();  var name = getName(); // A  updateName(name);}function getName() {  var name = app.first + ' ' + app.last; // B  return name;}
```

你已暂停A。按 Step into 时，开发者工具会执行这行代码，然后在 B 时暂停。

### 跳出代码行

如果在与您正在调试的问题无关的函数内暂停，请点击 Step out 图标 单步退出 以执行该函数的其余代码。

![1725186834097](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725186834097.png)

例如，假设您正在调试以下代码：

```
function updateHeader() {  var day = new Date().getDay();  var name = getName();  updateName(name); // C}function getName() {  var name = app.first + ' ' + app.last; // A  return name; // B}
```

你已暂停A。按下 Step out 后，开发者工具会在 getName() 中执行其余代码（在本例中为 B），然后在 C 时暂停。

### 运行特定行之前的所有代码

调试一个较长的函数时，可能会有大量与您要调试的问题无关的代码。

您可以单步调试所有行，但这可能会很繁琐。您可以在感兴趣的代码行上设置代码行断点，然后按 Resume Script Execution 继续执行脚本，但有更快的方法。

右键点击您感兴趣的代码行，然后选择前往此处。开发者工具会运行到目前为止所有代码，然后暂停在该行。

![1725186856504](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725186856504.png)

### 继续执行脚本

如需在暂停后继续执行脚本，请点击 Resume Script Execution 图标 继续执行脚本。开发者工具会执行脚本，直到下一个断点（如果有）。

![1725186867269](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725186867269.png)

### 强制执行脚本

若要忽略所有断点并强制脚本继续执行，请点击并按住 Resume Script Execution 图标 继续执行脚本，然后选择 Force 脚本执行 强制执行脚本。

![1725186878499](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725186878499.png)

### 更改会话串上下文

使用 Web Worker 或 Service Worker 时，点击 Threads 窗格中列出的上下文可切换到该上下文。蓝色箭头图标表示当前选定的上下文。

![1725186890346](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725186890346.png)

上方屏幕截图中的 Threads 窗格显示为蓝色。

例如，假设您在主线程和 Service Worker 脚本的断点处暂停。您想要查看 Service Worker 上下文的本地和全局属性，但 Sources 面板显示的是主脚本上下文。通过点击“Threads”窗格中的 Service Worker 条目，您可以切换到该上下文。

逐步执行以英文逗号分隔的表达式

> 要点：从 Chrome 108 版开始，调试程序可以逐步检查分号分隔 (;) 和逗号分隔 (,) 表达式。

您可以单步调试以英文逗号分隔的表达式，调试缩减后的代码。例如，请参考以下代码：

```
function foo() {}
function bar() {  foo();  foo();  return 42;}
bar();
```

缩减大小后，其中包含以英文逗号分隔的 foo(),foo(),42 表达式：

```
function foo(){}function bar(){return foo(),foo(),42}bar();
```

Debugger 会以相同的方式逐步检查此类表达式。

![1725186925363](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725186925363.png)

因此，步进行为是相同的：

1.缩减的代码和编写的代码之间。2.使用源代码映射根据原始代码调试缩减版代码时。换言之，当您看到分号时，即使您调试的实际源代码已缩减，您始终可以单步调试它们。

### 查看和修改本地属性、闭包属性和全局属性

在某行代码上暂停时，使用 Scope 窗格可以查看和修改局部作用域、闭包和全局作用域中的属性值和变量的值。

1.双击属性值可对其进行更改。2.不可枚举的属性显示为灰色。

![1725186937556](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725186937556.png)

上方屏幕截图中的 Scope（作用域）窗格显示为蓝色。

### 查看当前调用堆栈

在某行代码上暂停时，使用 Call Stack 窗格查看让您到达此点的调用堆栈。

点击某个条目即可跳转到调用该函数的代码行。蓝色箭头图标表示开发者工具当前突出显示的函数。

![1725186948812](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725186948812.png)

上方屏幕截图中的 Call Stack 窗格以蓝色勾勒。

> 注意：如果未在代码行暂停，Call Stack 窗格将为空。

### 重启调用堆栈中的函数（帧）

若要观察函数的行为并重新运行该函数，而不必重启整个调试流程，您可以在此函数暂停时重新开始执行单个函数。换言之，您可以在调用堆栈中重启函数的帧。

如需重启帧，请执行以下操作：

1.在断点处暂停函数执行。Call Stack 窗格记录了函数调用的顺序。2.在 Call Stack 窗格中，右键点击一个函数，然后从下拉菜单中选择 Restart frame。

![1725186961080](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725186961080.png)

注意：您可以重启调用堆栈中的任何函数帧，WebAssembly、异步函数和生成器函数除外。

如需了解 Restart frame 的工作原理，请考虑以下代码：

```
function foo(value) {    console.log(value);    bar(value);}

function bar(value) {    value++;    console.log(value);    debugger;}

foo(0);
```

foo() 函数将 0 作为参数，记录它并调用 bar() 函数。bar() 函数依次递增实参。

尝试通过以下方式重启这两个函数的帧：

1、将上面的代码复制到新的代码段中，然后运行该代码段。执行在 debugger 代码行断点停止。

> 注意：暂停执行时，请勿以编程方式更改调用堆栈帧的顺序。这可能会导致意外错误。

2、请注意，调试程序会在函数声明旁边显示当前值：value = 1。函数声明旁的当前值。

![1725186996688](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725186996688.png)

3、重启 bar() 帧。

![1725187007105](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187007105.png)

4、按 F9 逐步执行值增量语句。正在递增当前值。您会发现，当前值会增加：value = 2。

![1725187015748](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187015748.png)

5、（可选）在范围窗格中，双击相应值以进行修改，并设置所需的值。在“范围”窗格中修改该值。

![1725187055178](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187055178.png)

6、请尝试重启 bar() 帧并多次单步执行增量语句。该值继续增加。再次重新启动 bar() 帧。

![1725187067254](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187067254.png)

> 要点：为什么该值未重置为 0？

帧重启不会重置参数。换句话说，重启不会在调用函数时恢复初始状态。而只是将执行指针移到函数的开头。

因此，在同一函数重新启动后，当前参数值会保留在内存中。

1、现在，重启调用堆栈中的 foo() 帧。

![1725187078642](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187078642.png)

您会发现，该值仍然是 0。

![1725187087869](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187087869.png)

> 要点：为什么该值会重置为 0？

在 JavaScript 中，对参数的更改在函数之外不可见（反射）。嵌套函数接收值，而不是它们在内存中的位置。1. 继续执行脚本 (F8) 以完成本教程。

### 显示已列入忽略列表的帧

默认情况下，Call Stack 窗格仅显示与您的代码相关的帧，并省略添加到 设置。Settings > Ignore List 中的任何脚本。

![1725187097958](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187097958.png)

如需查看包含第三方框架的完整调用堆栈，请启用 Call Stack 部分下的 Show ignore-listed frame。

![1725187107917](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187107917.png)

请在此演示页面上试用：

1.在 Sources 面板中，打开 src > app > app.component.ts 文件。2.在 increment() 函数上设置一个断点。3.在 Call Stack 部分，选中或取消选中 Show ignore-listed framework 复选框，并观察调用堆栈中的相关帧或完整的帧列表。

### 查看异步帧

如果您正在使用的框架支持，则开发者工具可以通过将异步代码的两部分链接在一起来跟踪异步操作。

在这种情况下，调用堆栈会显示包括异步调用帧在内的所有通话记录。

![1725187117445](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187117445.png)

> 要点：开发者工具基于 console.createTask() API 方法实现此“异步堆栈标记”功能。实现 API 取决于框架。例如，Angular 支持此功能。

### 复制堆栈轨迹

右键点击 Call Stack 窗格中的任意位置，然后选择 Copy stack trace 以将当前调用堆栈复制到剪贴板。

![1725187127824](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187127824.png)

以下是输出结果示例：

```
getNumber1 (get-started.js:35)inputsAreEmpty (get-started.js:22)onClick (get-started.js:15)
```

### 浏览文件树

使用Page 窗格可浏览文件树。

文件树中由群组创作和部署的文件

> 注意：这是 Chrome 104 版及更高版本提供的 实验性。预览功能。

使用框架（例如 React 或 Angular）开发 Web 应用时，由于构建工具（例如 webpack 或 Vite）生成的文件较小，因此很难浏览源代码。

为了帮助您浏览来源，Sources > Page 窗格可以将文件分为两类：

1.代码图标。已编写。与您在 IDE 中查看的源文件类似。DevTools 会根据构建工具提供的源映射生成这些文件。2.“已部署”图标。已部署。浏览器读取的实际文件。这些文件通常会缩减大小。

如需启用分组功能，请在文件树顶部的三点状菜单下启用 三点状菜单。> Group files by Authored/Deployed 实验性。选项。

![1725187143563](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187143563.png)

在文件树中隐藏已列入忽略列表的来源

> 注意：这是 Chrome 106 及之后推出的一项 实验性。预览功能。

为便于您只关注自己创建的代码，默认情况下，Sources > Page 窗格会将添加到 设置。Settings > Ignore List 的所有脚本或目录灰显。

若要完全隐藏此类脚本，请依次选择来源 > 网页 > 三点状菜单。> 隐藏已忽略的来源。

![1725187152130](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187152130.png)

### 忽略某个脚本或脚本格式

忽略某个脚本，以便在调试时跳过它。如果脚本被忽略，则脚本在 Call Stack 窗格中会被遮盖，并且您在单步调试代码时永远不会进入脚本的函数。

例如，假设您正在单步调试此代码：

```
function animate() {  prepare();  lib.doFancyStuff(); // A  render();}
```

A 是您信任的第三方库。如果您确信正在调试的问题与第三方库无关，那么忽略该脚本就很合理。

### 忽略文件树中的某个脚本或目录

如需忽略单个脚本或整个目录，请运行以下命令：

1.在来源 > 网页中，右键点击某个目录或脚本文件。2.选择将目录/脚本添加到忽略列表。

![1725187169535](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187169535.png)

如果您没有隐藏已列入忽略列表的来源，则可以在文件树中选择此类来源，并在 警告。警告横幅上点击从忽略列表中删除或配置。

![1725187180358](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187180358.png)

否则，您可以从 设置。Settings > Ignore List 的列表中移除隐藏和忽略的目录和脚本。

### 在“编辑器”窗格中忽略脚本

如需从 Editor 窗格中忽略脚本，请执行以下操作：

1.打开相应文件。2.右键点击任意位置。3.选择将脚本添加到忽略列表。

![1725187192874](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187192874.png)

您可前往 设置。设置 > 忽略列表，从忽略列表中移除脚本。

### 忽略“Call Stack”窗格中的脚本

如需忽略 Call Stack 窗格中的脚本，请执行以下操作：

1.右键点击脚本中的某个函数。2.选择将脚本添加到忽略列表。

![1725187204241](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187204241.png)

您可前往 设置。设置 > 忽略列表，从忽略列表中移除脚本。

### 从任何页面运行调试代码段

如果您发现自己在控制台中反复运行相同的调试代码，不妨考虑使用代码段。代码段是您在开发者工具中编写、存储和运行的可执行脚本。

### 观察自定义 JavaScript 表达式的值

使用“Watch”窗格可监视自定义表达式的值。您可以监视任何有效的 JavaScript 表达式。

![1725187214451](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187214451.png)

1.点击 Add 表达式 添加表达式 以创建新的监视表达式。2.点击 Refresh 刷新 以刷新所有现有表达式的值。在单步调试代码时，值会自动刷新。3.将鼠标悬停在表达式上，然后点击删除表达式 删除表达式 即可将其删除。

### 检查和修改脚本

当您在 Page 窗格中打开脚本时，DevTools 会在 Editor 窗格中显示其内容。在 Editor 窗格中，您可以浏览和修改代码。

此外，您还可以在本地替换内容，或者创建一个工作区并将您在开发者工具中所做的更改直接保存到本地源中。

### 使缩减后的文件可读

默认情况下，Sources 面板会美观输出缩减大小的文件。经过整齐输出后，编辑器可能会以多行显示单个长代码行，使用 - 指示它是行的延续。

![1725187224784](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187224784.png)

如需在加载时查看缩小的文件，请点击编辑器左下角的 { }。

### 折叠代码块

若要折叠代码块，请将鼠标悬停在左侧列中的行号上，然后点击 收起。收起。

如需展开这段代码块，请点击它旁边的 {...}。



### 修改脚本

在修复错误时，您通常希望测试对 JavaScript 代码进行的一些更改。您无需在外部浏览器中进行更改，然后重新加载页面。您可以在开发者工具中修改脚本。

如需修改脚本，请执行以下操作：

1.在 Sources 面板的 Editor 窗格中打开文件。2.在 Editor 窗格中进行更改。3.按 Command + S (Mac) 或 Ctrl + S（Windows、Linux）进行保存。开发者工具会在 Chrome 的 JavaScript 引擎中修补整个 JS 文件。

![1725187237740](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187237740.png)

上方屏幕截图中的 Editor 窗格显示为蓝色。

### 实时修改已暂停的函数

> 注意：Chrome 105 版及更高版本提供此功能。

在执行暂停期间，您可以修改当前函数并实时应用更改，但存在以下限制：

1.您只能修改调用堆栈中最顶层的函数。2.不得以递归方式在堆栈中更靠下的位置对同一函数进行递归调用。

> 要点：当您应用更改时，调试程序会自动重启函数。因此，函数重启也适用。您无法重启 WebAssembly、async 和 generator 函数。如需实时修改函数，请执行以下操作：

1.使用断点暂停执行。2.修改已暂停的函数。3.按 Command / Ctrl + S 以应用更改。调试程序会自动重启函数。4.继续执行。

视频加载失败，请刷新页面再试[![img](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACIAAAAqCAMAAADhynmdAAAAQlBMVEUAAACcnJycnJycnJyoqKicnJycnJycnJycnJycnJyfn5+cnJydnZ2enp6kpKSdnZ2cnJyenp6cnJycnJycnJybm5t8KrXMAAAAFXRSTlMAyeb3CNp3tJRvHIEtJhBgqztWRJ+p5TqGAAABCklEQVQ4y5WTi27DIAxFAUMhgTzX8/+/urB2pdKI0x0pSoRuruyLbf7gF3PBaDE6X44LyY0D1SJQsfd9PpMM/CJx60v8SmV1HMSi1lKyA1n0jnwWSO08l04uJbxpBmTrpDtbGB6fmxC6Tc4BHv9aZDJdJsHW9w43Jez9x8T5M4l31WZsJn2bsYY+nUum2lQkGIVANPZ4FCLWOJImSTgjZE2SkU9crmu57mj9JBc93Qzj9R1d3HSG5bN5MRsnUzcGKK8Ns02z+Da7rYQE4bUE2PG1C6kVnkCyf0pwX8/jwbyxCLhcHpKTFkvkwK3pRmXtRrVFoTGYLvN+t0EUl0qrRaF1pFBz0anp/ptvNB4SY1XDAVMAAAAASUVORK5CYII=) 刷新](javascript:void(0);)

在此示例中，addend1 和 addend2 变量最初的 string 类型不正确。因此，不是将数字相加，而是串联字符串。为了解决此问题，我们在实时编辑期间添加了 parseInt() 函数。

### 搜索和替换脚本中的文本

如需搜索脚本中的文本，请执行以下操作：

1、 在 Sources 面板的 Editor 窗格中打开文件。2、 若要打开内置搜索栏，请按 Command+F (Mac) 或 Ctrl+F（Windows、Linux）。3、 在栏中输入您的查询内容。

![1725187270702](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187270702.png)

您可以选择：

1.点击 匹配大小写。匹配大小写以使查询区分大小写。2.点击 正则表达式按钮。使用正则表达式，即可使用正则表达式进行搜索。

4、按 Enter 键。若要跳转到上一个或下一个搜索结果，请按向上或向下按钮。

如需替换您找到的文字，请执行以下操作：

1、 在搜索栏上，点击 替换。Replace 按钮。替换。

![1725187281682](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725187281682.png)

2、 输入要替换的文本，然后点击替换或全部替换。