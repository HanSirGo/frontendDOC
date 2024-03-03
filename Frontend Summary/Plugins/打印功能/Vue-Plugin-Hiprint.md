## Vue-Plugin-Hiprint

> 原文：https://juejin.cn/post/7297080018655412250
>
> github 地址：https://github.com/CcSimple/vue-plugin-hiprint?tab=readme-ov-file

```
Vue-Plugin-HiPrint 是一个Vue.js的插件，旨在提供一个简单而强大的打印解决方案。通过 Vue-Plugin-HiPrint，您可以轻松地在Vue.js应用程序中实现高度定制的打印功能。
```

```
1. 安装 Vue-Plugin-HiPrint

npm install vue-plugin-hiprint --save
# 或
yarn add vue-plugin-hiprint
```

```js
2. 在 Vue 项目中引入 Vue-Plugin-HiPrint

// main.js中引入
import Vue from 'vue';
import VuePluginHiPrint from 'vue-plugin-hiprint';

Vue.use(VuePluginHiPrint);
```

```
3. 创建打印模板
在开始打印之前，您需要创建打印模板。Vue-Plugin-HiPrint 使用 HiPrint 作为底层打印引擎，它支持使用 HTML 和 CSS 创建高度自定义的打印模板。您可以创建一个包含您想要打印的内容的 HTML 模板，然后使用 CSS 样式进行格式化。

请去 demo预览 ： https://ccsimple.gitee.io/vue-plugin-hiprint/ "https://ccsimple.gitee.io/vue-plugin-hiprint/ 里创建一个适合业务需求的打印模板：
```

![1709454812827](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709454812827.png)

```
4. 在 Vue 组件中使用打印

	一旦您创建了打印模板，您可以在您的 Vue 组件中使用 Vue-Plugin-HiPrint 来触发打印操作。首先，先要在项目的index.html文件中引入print-lock.css样式文件，这个文件在node_modules/vue-plugin-hiprint/dist/目录。
```

```html
// 注意：需复制一份print-lock.css样式文件放到与index.html同级目录下，否则打印样式有问题。
<!--【必须】在index.html 文件中添加打印所需样式(此cdn可能不稳定):-->
<link rel="stylesheet" type="text/css" media="print" href="https://cdn.jsdelivr.net/npm/vue-plugin-hiprint@latest/dist/print-lock.css">
<!-- 推荐使用：可以调整成 相对链接/自有链接, 【重要】名称需要一致 【print-lock.css】-->
<link rel="stylesheet" type="text/css" media="print" href="/print-lock.css">
```

```
接下来，是在组件中的使用方法：

import {hiPrintPlugin } from 'vue-plugin-hiprint'
hiPrintPlugin.disAutoConnect();  //取消自动打印直接连接客户端
hiprint.init(); 

组件中我们需要先取消它的自动连接客户端打印功能，然后初始化vue-plugin-hiprint。
```

```js
6. 自定义打印模板
	Vue-Plugin-HiPrint 允许您自定义打印样式，以满足您的具体需求。您可以在 预览网站[1]中设计好需要的样式并复制自定义模板的JSON数据，在项目中新建mb.json文件将模板json数据粘贴进去。
	
// 使用模板:

import mb from './mb.json'
function orderPrint(){
let printData = {orderId:'单号',title:'模板标题',table:[{NAME:'表格数据'}]};
let hiprintTemplate = new hiprint.PrintTemplate({ template: mb});
// 打印
hiprintTemplate.print(printData);
}
在上面的示例中，我们使用import引入了自定义模板，使用printData自定义了表单具体数据，最后使用print方法完美实现了打印。
```

