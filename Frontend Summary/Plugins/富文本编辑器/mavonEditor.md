# **mavonEditor**

**mavonEditor**是一个用于Vue.js的Markdown编辑器。它是一款轻量级的富文本编辑器，将Markdown和vue.js无缝结合，帮助开发者快速创建出优美的富文本内容。

![1714282936923](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282936923.png)

**资源获取**

| GitHub | https://github.com/hinesboy/mavonEditor                      |
| ------ | ------------------------------------------------------------ |
| npm    | https://www.npmjs.com/package/mavon-editor/v/next?activeTab=readme |



**Markdown语法指南**

Markdown语法指南，包括如何创建标题、列表、链接、图片、代码块等等的说明，非常适合Markdown的初学者和进阶者。

参考文档：

1. https://www.markdownguide.org/basic-syntax/

2. https://markdown.com.cn/basic-syntax/

   ![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282952987.png)

   

   ![1714282970586](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282970586.png)



**适用场景**

| Vue2     | ![图片](https://mmbiz.qpic.cn/mmbiz_svg/LXqicVqwJiatrsiarATGHGOqd5Aia317HVThl4EJy31iccC1Rcpuw6ibk2ELIJianFVvZgErCygdAPHQ2Esd1ZUIuDkraMBmJD8twug/640?wx_fmt=svg&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1) |
| -------- | ------------------------------------------------------------ |
| **Vue3** | ![图片](https://mmbiz.qpic.cn/mmbiz_svg/LXqicVqwJiatrsiarATGHGOqd5Aia317HVThSmhb7Youkiaybciby3Fib8BlBQT532cFyTgTkGBnNSYj2TGiaN9yUgrKVyztYmpW6069/640?wx_fmt=svg&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1) |

![1714282979380](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282979380.png)

**推荐理由**

1. **实时预览：**在编辑过程中，mavonEditor提供了一个实时预览的功能，你可以在编写的同时看到文章的实时展示效果；

2. **Markdown编辑：**mavonEditor是一款Markdown编辑器，它支持标准的Markdown语法，包括标题、列表、引用、代码块、图片等，使你的文章结构更清晰；

3. **轻量级：**mavonEditor是一个轻量级的编辑器，它不会给你的项目增加太多负担，同时它的使用非常简单，你可以快速地将它集成到你的项目中；

4. **图片上传和管理：**mavonEditor支持图片的上传和管理，你可以方便地将图片插入到你的文章中，并且它支持多种图片存储方式，包括七牛云、阿里云等。

5. **代码高亮：**mavonEditor支持代码高亮，它可以识别多种语言的代码，并且为其添加高亮，使你的代码更易读；

6. **可扩展性：**mavonEditor有很好的可扩展性，你可以根据你的需求自定义编辑器的功能和样式，使它更符合你的需求；

7. **兼容性：**mavonEditor兼容主流的浏览器，包括Chrome、Firefox、Safari等，你无需担心浏览器的兼容问题。

   

![1714282989287](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282989287.png)

**使用说明**

配置参考：https://www.npmjs.com/package/mavon-editor/v/next?activeTab=readme

**安装**

```
npm install mavon-editor@next --save
```

**配置**

```js

import { createApp } from "vue";
import App from "./App.vue";
import { createPinia } from "pinia";
import router from "./router";
import "./assets/css/base.scss";
// 全局注册
// import with ES6
import mavonEditor from 'mavon-editor'
import 'mavon-editor/dist/css/index.css'

const pinia = createPinia();
const app = createApp(App);
app.use(mavonEditor);
app.use(router);
app.use(pinia);
app.mount("#app");
```

**页面使用**

![1714283004698](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714283004698.png)

**效果预览**

![1714283015534](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714283015534.png)

**总结**

通过以上的特性描述和案例演示，我们可以看出，mavonEditor是一款非常强大和实用的Markdown编辑器，无论你是个人博客的作者，还是一个开发者，都可以通过mavonEditor快速地创建出优美的富文本内容。

![1714283032594](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714283032594.png)