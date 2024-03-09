#### 一、Css书写顺序：
 - 1.位置属性(position, top, right, z-index, display, float等)　
 - 2.大小(width, height, padding, margin)
 - 3.文字系列(font, line-height, letter-spacing, color- text-align等)　
 - 4.背景(background, border等)
 - 5.其他(animation, transition等)
#### 二、Css语法：

 - 命名一般为小写英文字母。
 - 为了代码的易读性，在每个声明块的左花括号前添加一个空格。
 - 每条声明语句的 : 后应该插入一个空格。
 - 所有声明语句都应当以分号结尾。最后一条声明语句后面的分号是可选的，但是，如果省略这个分号，你的代码可能更易出错。
 - 对于属性值或颜色参数，省略小于 1 的小数前面的 0 （例如，.5 代替 0.5；-.5px 代替 -0.5px）。
 - 十六进制值应该全部小写，例如，#fff。
 - 尽量使用简写形式的十六进制值，例如，用 #fff 代替 #ffffff。
 - 避免为 0 值指定单位，例如，用 margin: 0; 代替 margin: 0px;。

#### 三、Css的引入：
Css的引入一般有两种，link和@import,一般建议使用link引入。这样可以避免考虑@import的语法规则和注意事项，避免产生资源文件下载顺序混乱和http请求过多的烦恼。
#### 四、Css的命名规范(BEM,OOCSS)：
##### BEM

> 什么是BEM：BEM的意思就是块（block）、元素（element）、修饰符（modifier）,是由Yandex团队提出的一种前端命名方法论。这种巧妙的命名方法让你的CSS类对其他开发者来说更加透明而且更有意义。

**命名约定如下：**

 - .block{} // 块即是通常所说的 Web 应用开发中的组件或模块。每个块在逻辑上和功能上都是相互独立的。
 - .block__element{} // 元素是块中的组成部分。元素不能离开块来使用。BEM 不推荐在元素中嵌套其他元素。
 - .block--modifier{} // 修饰符用来定义块或元素的外观和行为。同样的块在应用不同的修饰符之后，会有不同的外观。

**优点：**
BEM 的优点在于所产生的 CSS 类名都只使用一个类别选择器，可以避免传统做法中由于多个类别选择器嵌套带来的复杂的属性级联问题。在 BEM 命名规则中，所有的 CSS 样式规则都只用一个类别选择器。因此所有样式规则的特异性（specificity）都是相同的，也就不存在复杂的优先级问题。这可以简化属性值的层叠规则。代码清单中的命名规则的好处在于每个 CSS 类名都很简单明了，而且类名的层次关系可以与 DOM 节点的树型结构相对应。

**缺点：**
这样类名过于长，且复杂。

##### OOCSS

> 什么是OOCSS（面向对象CSS）：
> 
> OOCSS 表示的是面向对象 CSS（Object Oriented CSS），是一种把面向对象方法学应用到 CSS
> 代码组织和管理中的实践。
> OOCSS最关键的一点就是：提高他的灵活性和可重用性。这个也是OOCSS最重要的一点。OOCSS主张是通过在基础组件中添加更多的类，从而扩展基础组件的CSS规则，从而使CSS有更好的扩展性。
###### 原生
**有两个大小 ，外边距相同的div。**
```javascript
.box1{
    width: 30%;
    margin-left: 35%;
    height: 300px;
    margin-top: 50px;
    text-align: center;
    line-height: 300px;
    color: white;
    background-color: #8A2BE2;
    font-size: 22px;
}

.box2{
    width: 30%;
    margin-left: 35%;
    height: 300px;
    margin-top: 50px;
    text-align: center;
    line-height: 300px;
    color: white;
    background-color: blue;
    font-size: 23px;
}
```
但是，其中有很多代码是相同的， 所以上边这种写法会增加很多工作量而且也不便于维护，所以一般我们也会这样写：
```javascript
.box1,.box2{
    width: 30%;
    margin-left: 35%;
    height: 300px;
    margin-top: 50px;
    text-align: center;
    line-height: 300px;
    color: white;
}

.box1{
    background-color: #8A2BE2;
    font-size: 22px;
}

.box2{
    background-color: blue;
    font-size: 23px;
}
```

> 这样把重复的css代码拿出来，单独写一个样式表，然后将有相同样式的类添加到这个样式里就行，
> 
> 但是后期如果再增加一个相同的div的时候就需要重新进入样式表添加类，如果是大型网站的话
> 这样也会降低我们的工作效率，所以我们这时候就需要利用OOCSS写法来降低维护难度及工作量。
> 
> 写法很简单，只要将重复的样式拿出来单独写一个类，然后将类嵌入到HTML代码中就可以了，

###### OOCSS

```javascript
.box1{
    background-color: #8A2BE2;
    font-size: 22px; 
}

.box2{
    background-color: blue;
    font-size: 23px;
}

.oocss1{
    width: 30%;
    margin-left: 35%;
    height: 300px;
    margin-top: 50px;
    text-align: center;
    line-height: 300px;
    color: white;
}
```

**OOCSS的优点：**
减少CSS代码。
具有清洁的HTML标记，有语义的类名，逻辑性强的层次关系。
语义标记，有助于SEO。
更好的页面优化，更快的加载时间（因为有很多组件重用）。
可扩展的标记和CSS样式，有更多的组件可以放到库中，而不影响其他的组件。
能轻松构造新的页面布局，或制作新的页面风格。

**OOCSS的缺点：**
OOCSS适合真正的大型网站开发，因为大型网站用到的可重用性组件特别的多，如果运用在小型项目中可能见不到什么成效。所以用不用OOCSS应该根据你的项目来决定。如果没用巧妙的使用，创建组件可能对于你来说是一堆没用的东西，成为一烂摊子，给你的维护带来意想不到的杯具，说不定还是个维护的噩梦。
#### 五、css modules
##### 为什么引入CSS Modules
或者可以这么说，CSS Modules为我们解决了什么痛点。针对以往我写网页样式的经验，具体来说可以归纳为以下几点：
###### 全局样式冲突
过程是这样的：你现在有两个模块，分别为A、B,你可能会单独针对这两个模块编写自己的样式，例如a.css、b.css，看一下代码
```javascript
// A.js
import './a.css'
const html = '<h1 class="text">module A</h1>'

// B.js
import './b.css'
const html = '<h1 class="text">module B</h1>'
```

下面是样式：
```javascript
/* a.css */
.text {
    color: red;
}

/* b.css */
.text {
    color: blue;
}
```
导入到入口APP中
```javascript
// App.js
import A from './A.js'
import B from './B.js'

element.innerTHML = 'xxx'
```
由于样式是统一加载到入口中，因此实际上的样式合在一起（这里暂定为mix.css）显示顺序为：
```javascript
/* mix.css */

/* a.css */
.text {
    color: red;
}

/* b.css */
.text {
    color: blue;
}
```
根据CSS的Layout规则，因此后面的样式会覆盖掉前面的样式声明，最终有效的就是
```javascript
text
```
的颜色为
```javascript
blue
```
的那条规则，这就是全局样式覆盖，同理，这在
```javascript
js
```
中也同样存在，因此就引入了模块化，在js中可以用立即执行函数表达式来隔离出不同的模块
```javascript
var moduleA = (function(document, undefined){
    // your module code
})(document)

var moduleB = (function(document, undefined){
    // your module code
})(document)
```
而在css中要想引入模块化，那么就只能通过
```javascript
namespace
```
来实现，而这个又会带来新的问题，这个下面会讲到
嵌套层次过深的选择器
为了解决全局样式的冲突问题，就不得不引入一些特地命名
```javascript
namespace
```
来区分
```javascript
scope
```
，但是往往有些
```javascript
namespace
```
命名得不够清晰，就会造成要想下一个样式不会覆盖，就要再加一个新的
```javascript
namespace
```
来进行区分，最终可能一个元素最终的显示样式类似如以下：
```javascript
.widget .table .row .cell .content .header .title {
  padding: 10px 20px;
  font-weight: bold;
  font-size: 2rem;
}
```
**在上一个元素的显示上使用了7个选择器，总结起来会有以下问题：**
 - 根据CSS选择器的解析规则可以知道，层级越深，比较的次数也就越多。当然在更多的情况下，可能嵌套的层次还会更深，另外，这里单单用了类选择器，而采用类选择器的时候，可能对整个网页的渲染影响更重。
 - 增加了不必要的字节开销
 - 语义混乱，当文档中出现过多的
```javascript
content
```
、
```javascript
title
```
以及
```javascript
item
```
这些通用的类名时，你可能要花上老半天才知道它们到底是用在哪个元素上
 - 可扩展性不好，约束越多，扩展性越差

 ###### 一些解决方案
 **CSS预处理器(Sass/Less等)**--不可避免的会出现冲突样式
 **BEM（Block Element Modifier）**
##### 什么是 CSS Modules？
> CSS Modules 既不是官方标准，也不是浏览器的特性，而是在构建步骤（例如使用 Webpack 或 Vite）中对 CSS
> 类名和选择器限定作用域的一种方式（类似于命名空间）。

比如我们在代码中写如下代码
```javascript
import styles from "./styles.css";

element.innerHTML =
  `<h1 class="${styles.title}">
     An example heading
   </h1>`;
```
最后编译出来的的结果为
```javascript
<h1 class="_styles__title_309571057">
  An example heading
</h1>

<style>
._styles__title_309571057 {
  background-color: red;
}
</style>
```

这样可以通过一定的规则将 class 的名称变化这样就可以避免不同组件之间的 css 冲突。
##### CSS Modules 解决了什么问题

 - 全局命名冲突，因为CSS Modules只关心组件本身，只要保证组件本身命名不冲突，就不会有这样的问题，一个组件被编译之后的类名可能是这样的：
```javascript
/* App.css */
.text {
    color: red;
}

/* 编译之后可能是这样的 */
.App__text___3lRY_ {
    color: red;
}
```
命名唯一，因此保证了全局不会冲突。

 - 模块化
可以使用 **composes** 来引入自身模块中的样式以及另一个模块的样式：
**1.来引入自身模块中的样式**
```javascript
.serif-font {
  font-family: Georgia, serif;
}

.display {
  composes: serif-font;
  font-size: 30px;
  line-height: 35px;
}
```
应用到元素上可以这样使用：
```javascript
import type from "./type.css";

element.innerHTML = 
  `<h1 class="${type.display}">
    This is a heading
  </h1>`;
```
之后编译出来的模板可能是这样的：

```javascript
<h1 class="Type__display__0980340 Type__serif__404840">
  Heading title
</h1>
```
**2.从另一个模块中引入，可以这样写：**
```javascript
.element {
  composes: dark-red from "./colors.css";
  font-size: 30px;
  line-height: 1.2;
}
```
 - **解决嵌套层次过深的问题**
因为CSS Modules只关注与组件本身，组件本身基本都可以使用扁平的类名来写，类似于这样的：
```javascript
.root {
  composes: box from "shared/styles/layout.css";
  border-style: dotted;
  border-color: green;
}

.text {
  composes: heading from "shared/styles/typography.css";
  font-weight: 200;
  color: green;
}
```
##### 与scoped比较
 - scoped可以隔离本页面的样式，但是如何引入的子组件依然会收到覆盖样式的影响。
 - css modules即使引入组件也不会相互影响。
 - scoped可以嵌套的写样式，逻辑层次更加清晰。
 - css modules通常是没有嵌套的，写在了最外层。

> **CSS Modules 与 scoped只能用一个**
##### CSS Modules 用法
CSS Modules 用法 地址：[阮一峰博客教程](https://www.ruanyifeng.com/blog/2016/06/css_modules.html)
###### 配置 webpack.config.js。

```javascript
module.exports = {
  entry: __dirname + '/index.js',
  output: {
    publicPath: '/',
    filename: './bundle.js'
  },
  module: {
    loaders: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        loader: 'babel',
        query: {
          presets: ['es2015', 'stage-0', 'react']
        }
      },
      // {
      //   test: /\.css$/,
      //   loader: "style-loader!css-loader?modules"
      // },
      // 
      {
        // 这里是匹配的文件名称（此处假设我需要 scopedcss 的 css文件: a.scopedcss）
        test: /\.scopedcss$/,  
        loader: "style-loader!css-loader?modules" // 这里是使用对用的loader处理
      },
    ]
  }
};

```
![1709778119281](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709778119281.png)
**自定义编译后的样式哈希值**

```javascript
  {
    test: /\.scopedcss$/,
    loader: "style-loader!css-loader?modules&localIdentName=[path][name]---[local]---[hash:base64:5]123456789"
  },

```
![1709778161506](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709778161506.png)
