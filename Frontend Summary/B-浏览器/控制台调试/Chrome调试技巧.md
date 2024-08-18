# 前端Chrome调试技巧最全汇总

![1721466131582](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466131582.png)

注：本文测试、截图均为Edge浏览器（内核是Chromium），浏览器内核可了解《**有哪些浏览器/内核？**[1]》

## 00、基础操作汇总

| **操作类型**                        | **快捷键/说明**                                              |
| :---------------------------------- | :----------------------------------------------------------- |
| 切换浏览器标签                      | 🔸 `Ctrl+1到8`切换到对应序号的浏览器标签 🔸 `Ctrl+PgUp/PgDn`标签页左右切换 |
| 浏览器全屏                          | `F11` (`⌘ + shift + F` Mac)                                  |
| 打开调试模式                        | 🔸 `F12`，`Ctrl + Shift + I` (Windows) 或` Cmd + Opt + I` (Mac) 🔸 页面右键菜单“检查”，浏览器菜单“开发者工具” |
| 切换调试工具位置（下面、右边）      | `ctrl + shift + D` (`⌘ + shift + D` Mac)                     |
| 切换 DevTools 的面板标签            | `ctrl + [` 和 `ctrl + ]`左右切换调试工具面板                 |
| 内容搜索查找                        | `Ctrl+F`：元素、控制台、源代码、网络都支持搜索查找           |
| 使用命令Command面板                 | `Ctrl] + [Shift] + [P]` （Mac：` [⌘] + [Shift]+ [P]`） 类似VSCode的命令面板，有大量的隐藏功能，可以在这里搜索使用 |
| 复制元素                            | 🔸 元素面板：选中元素》`Ctrl+C` 🔸 元素面板：选中元素》右键菜单》复制元素 🔸 `copy($0)` 控制台中代码复制当前选中元素 |
| 控制台：快速访问当前元素'$0'        | `$0`代表在元素面板中选中元素，`$1`是上一个，支持到`$4`       |
| 控制台：全局`copy`方法              | 复制任何对象到剪切板，`copy(window.location)`                |
| 控制台：queryObjects(Type)          | 查询指定类型（构造器）的对象实例有哪些                       |
| 控制台：保存堆栈信息( Stack trace ) | 报错信息可以右键另存为文件，保存完整堆栈信息                 |
| 控制台：`$`、`$$`查询Dom元素        | - `$` = `document.querySelector` - `$$` = `document.querySelectorAll` |
| Store as global（存储为全局变量）   | 🔸 控制台（右键）：把一个对象设置为全局变量，自动命名为`temp*` 🔸 元素面板（右键）：把一个元素设置全局变量，同上 |
| 元素：`h`快速隐藏、显示该元素       | 选中元素，按下`h`快速隐藏、显示该元素。                      |
| 元素：移动元素                      | 🔸 鼠标拖动到任意位置 🔸 上下移动，`[ctrl] + [⬆]` / `[ctrl] + [⬇]` （`[⌘] + [⬆] / [⌘] + [⬇]`on Mac） |
|                                     |                                                              |

**🔸Store as global（存储为全局变量）**：类似copy方法，将一个对象保存为全局变量，变量命名依次为`temp1`、`temp2`。

![1721466152217](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466152217.png)

**🔸保存堆栈信息( Stack trace )**：错误堆栈信息另存为文件，保存完整堆栈信息。

![1721466163704](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466163704.png)

**🔸Command面板**：同VSCode的命令面板，可以找到调试工具的所有的（隐藏）功能。`Ctrl] + [Shift] + [P]`（Mac：` [⌘] + [Shift]+ [P]`）

![1721466179423](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466179423.png)

- **设置主题**：打开Command面板，搜索“主题”，可以切换多种主题

  ![1721466195432](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466195432.png)

- **分析代码的覆盖率**：打开Command面板，如下图搜索“覆盖”，分析首次页面加载过程中哪些代码执行了，那些没有执行，输出一个报告。

![1721466213391](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466213391.png)

选择文件可进一步查看代码的使用分析，红色 = 尚未执行，青蓝色 = 至少执行了一次。

![1721466225428](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466225428.png)

**🔸元素截图**：输出指定元素的截图，包含隐藏滚动的内容，这个功能挺好用的。

- 通过Command面板：搜索“截图”，全屏截图、指定节点截图。
- 元素面板中，选中元素》右键菜单》“捕获节点屏幕截图”。

![1721466239524](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466239524.png)

## 01、元素面板（Element）

可以自由调试DOM、CSS样式，使用评率级高：⭐⭐⭐⭐⭐。

- **DOM树**：左侧为DOM元素树，支持多种操作，如编辑、删除、新增、复制DOM元素，更多可查看右键菜单。
- **样式**：选中元素的样式，可编辑、添加CSS样式，实时预览。
- **已计算**：选中元素计算的样式值。
- **布局**：grid布局、flex布局调试。

![1721466254371](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466254371.png)

### 1.1、CSS可视化编辑器

可视化的颜色、贝塞尔曲线、阴影编辑器，所见即所得，挺好用！

- **颜色编辑器**：只要是颜色属性，都可以点击色块按钮可视化编辑颜色，支持拖动设置颜色、取色、设置对比度。
- **Grid、Flex布局编辑器**：当使用弹性布局Grid、Flex时，支持可视化编辑布局对齐方式。
- **阴影编辑器**：阴影`shadow`属性上，会出现编辑器按钮，可视化编辑阴影效果。

![1721466269441](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466269441.png)

- **贝塞尔曲线编辑器**：在动画`transition`、`animation`中会用到贝塞尔曲线函数（缓动函数）。

![1721466284502](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466284502.png)

### 1.2、强制激活伪类

强制激活元素的伪类效果。

- 选择Dom节点右键“强制状态”。
- 如下图CSS样式中“切换元素状态”。

![1721466301001](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466301001.png)

### 1.3、DOM断点

选中DOM元素，右键设置中断点，可以在元素更改（JS代码修改DOM）时触发断点。

![1721466312734](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466312734.png)

添加后可以在源代码中查看到已添加的DOM断点，或者元素面板中的“DOM断点”。

![1721466326750](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466326750.png)

## 02、控制台面板（Console）

主要功能就是调试JavaScript代码，是比较常用的调试工具，使用评率很高：⭐⭐⭐⭐⭐。

- **运行代码**：可执行任意JS代码，包括调用页面已有的JS对象、函数。
- **console输出**：代码中的Console、异常错误都会在这里输出。

![1721466342788](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466342788.png)

### 2.1、console函数

用console函数输出变量，是比较常用的调试技巧，console的常用函数：

| **console**[2]**函数**              | **说明**                                                     |
| :---------------------------------- | :----------------------------------------------------------- |
| console.**log**(str)                | 控制台输出一条消息                                           |
| console.**error**(str);             | 打印一条错误信息，类似的还有`info`、`warn`                   |
| console.**table**(data [, columns]) | 将数据以表格的形式显示，很实用，data为数组或对象，第二个参数（数组）可指定输出的列 |
| console.**dir**(object)             | 树形方式打印对象，特别是DOM对象非常实用                      |
| console.**assert**(false, 'false')  | 断言输出，为`false`才会输出                                  |
| console.**trace**()                 | 输出当前位置的执行堆栈，用断点会更实用一些。                 |
| console.**time**(label)             | 计时器，可用来计算耗时（毫秒），三个函数配合使用：**time**(开始计时) > **timeLog**(计时) > **timeEnd**(结束) |
| console.**clear**()                 | 清空控制台，并输出 Console was cleared。                     |

```
console.log('log');
console.info('info');
console.warn('warn');
console.error('error');
console.table(["sam", "egan", "kettle"])
throw new Error('出错了！！！')
```

控制台输出效果，右侧链接为对应JS代码的链接。

![1721466366705](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466366705.png)

用`console.time()`来计算代码的耗时，参数为计时器命名。

```js
function sum(n) {
    let sum = 0;
    for (i = 1; i <= n; i++) {
        let u = {name: 'sam', age: i}
        sum += i;
    }
    return sum
}

// 计算一个函数的耗时
console.time('sum') // 开始计时
const total = sum(100000);
console.timeLog('sum');  // 计时：sum: 4.43994140625 ms
const total2 = sum(1000);
console.timeEnd('sum');  // 计时：sum: 5.0419921875 ms
console.log({total});  //{total: 5000050000}
```

### 2.2、增强log：真实用！

```js
const x = 100, y = 200;
console.log(x, y); // 100 200
console.log({x, y}); // {x: 100, y: 200}
console.table({x, y}); 
```

如上代码，经常我们会输出一些变量值，如果直接输出值，则不知道值对应的变量。这时可以用字面量对象，输出的可读性立马就提升了，再加上`console.table`就更完美了。

![1721466392084](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466392084.png)

### 2.3、自定义log样式：占位符

`console`函数支持的占位符：

| 占位符       | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| `%c`         | CSS样式占位符，值就是CSS样式，如下示例，可用来自定义log的样式 |
| `%o` or `%O` | 打印 JavaScript 对象。在审阅器点击对象名字可展开更多对象的信息。 |
| `%d` or `%i` | 打印整数。支持数字格式化。例如，console.log("Foo %.2d", 1.1) 会输出有先导 0 的两位有效数字：Foo 01。 |
| `%s`         | 打印字符串。                                                 |
| `%f`         | 打印浮点数。支持格式化，比如 console.log("Foo %.2f", 1.1) 会输出两位小数：Foo 1.10 |

![1721466406603](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466406603.png)

### 2.4、监听函数

通过如下（调试工具）的全局函数可监听一个函数、事件的执行。

| 函数                        | 说明                                                   |
| :-------------------------- | :----------------------------------------------------- |
| **monitor**( function )     | 监听一个函数，当被监听函数执行的时候，会打印被调用信息 |
| **monitorEvents** ( event ) | 监听一个事件，当事件被触发时打印触发事件日志           |

![1721466423651](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466423651.png)

### 2.5、监听变量：活动表达式

一个不错的小功能，点击下图中的眼睛图标，相当于添加了一个动态表达式，然后实时监控（显示）该表达式的值。

![1721466436973](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466436973.png)

## 03、源代码面板（Sources）

顾名思义，管理网页所有的源代码文件，包括JS、CSS、图片等资源，经常就是在这里断点调试JS代码，使用评率中：⭐⭐⭐⭐。

在调试模式下可以查看（鼠标悬浮在变量上）变量值、上下文作用域、函数调用堆栈信息。

![1721466452189](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466452189.png)

- **① 页面资源目录**：页面涉及的所有资源（树）。
- **② 源代码**：文件源代码，可以在这里添加断点调试JS代码，如果想编辑在线JS代码，也是可以的，详见后文。
- **③ Debug工具栏**：断点调试的操作工具，可以控制暂停、继续、单步...执行代码。
- **④ 断点调试器**：这里包含多个断点调试器：

> - **监视（Watch）**：可添加对变量的监视。
> - **断点（Breakpoints）**：所有添加的断点，可在这里编辑、删除管理。
> - **作用域（Scope）**：当前代码上下文的作用域，含闭包。
> - **调用堆栈（Call Stack**）：当前函数的调用堆栈，推荐参考《**JavaScript函数(2)原理{深入}执行上下文**[3]》。
> - **XHR/提取断点**：可以在这里添加对AJAX请求（XHR、Fetch）的断点，添加URL地址即可。
> - **DOM断点**：在元素页面添加的DOM断点会在这里显示。

### 3.1、断点调试

如下图，在源代码行号位置添加断点。

![1721466466286](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466466286.png)

- **添加断点（Add breakpoint）**：添加一个普通断点，最左侧行号处，点击添加断点，或者右键菜单。

也可以在JS代码中设置断点，关键字：`debugger`

```
debugger // 会在这里断点
console.log('debugger')
```

- **添加条件断点（Add conditional breakpoint）**：添加一个条件断点，符合条件断点才会生效，条件可使用当前代码上下文中的变量。

![1721466482371](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466482371.png)

- **添加记录点（Add logpoint）**：添加一个日志打印，打印当前代码环境的变量，非常方便，不用在源码下添加`console`了。

![1721466497213](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466497213.png)

### 3.2、调试线上代码：本地工作区

如果是线上的环境，能不能直接修改源代码（JS、CSS）调试呢？—— 可以的。思路就是创建本地的JS副本，页面加载本地的JS文件，就可以在本地JS文件上修改了。

**① 创建本地工作目录**：

- 如下图，源代码下面找到“覆盖（Override）”。

![1721466508848](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466508848.png)

- 然后点击“选择替代文件夹”，添加一个本地空的文件夹，作为本地工作目录。
- 添加后要注意要确认授权。

![1721466524190](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466524190.png)

**② 创建源代码的本地副本**：选择需要修改的源代码右键“保存以备替代”，就会在本地目录创建副本文件，网页使用本地的JS文件。

- 创建的本地副本，可以在“覆盖”下看到，也可以在本地文件夹下看到。

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)![1721466544319](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466544319.png)

**③ 编辑代码**：该JS文件就可以直接在源代码中编辑修改了，实时生效。

- CSS、HTML、JavaScript都支持。
- 可以在浏览器的源代码中修改，也可以本地其他工具中打开编辑。

![1721466559769](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466559769.png)

## 04、网络面板（Network）

![1721466573365](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466573365.png)

工具栏中两个比较实用的小功能：禁用缓存、模拟弱网环境。

![1721466586979](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466586979.png)

点击请求的资源项，可以查看详细的请求、响应数据，常用于服务端接口的联调。

![1721466606959](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466606959.png)

还可以编辑参数，重新发起请求

![1721466625970](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466625970.png)

## 05、性能面板（Performance）

先录制，后分析，分析网络、CPU、内存、渲染FPS帧率，用于定位、解决页面性能问题。

![1721466646475](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466646475.png)

> **🚩特别提示**：调试性能建议在**无恒模式**下进行，尽量避浏览器插件的影响。包括其他异常Bug的调试，也要考虑浏览器插件问题，之前就遇到过类似问题，页面上一个Bug怎么也复现不了，几经波折才发现是测试机上的油猴插件的影响。

**🔸性能监视器**（Performance monitor）面板可以**实时**的监控页面性能参数。

![1721466659540](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466659540.png)

**🔸Lighthouse**，这个就很厉害了，对页面进行综合分析，包括性能、PWA（Progressive Web App，渐进式Web应用）、SEO、无障碍访问等，分析完后产出报告，给出得分，还给出了页面改进建议。

![1721466670149](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721466670149.png)

## 参考资料

- 掘金小册：**你不知道的 Chrome 调试技巧**[4] ，**开源版**[5]
- 掘金小册：**前端开发调试之 PC 端调试**[6]
- **前端必须知道的开发调试知识.pptx**[7]
- **有哪些浏览器/内核？**[8]
- **JavaScript函数(2)原理{深入}执行上下文**[9]