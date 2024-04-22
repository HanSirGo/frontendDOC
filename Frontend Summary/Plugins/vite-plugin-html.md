## vite-plugin-html

### 使用vite-plugin-html向html模板注入内容

在有些时候，我们的网页要做出一些seo的配置，如title、description、keywords等，如果我们想后台自定义这些内容，则需要借助vite-plugin-html插件，调用相关接口获取内容向html文件注入。步骤如下：

1、安装依赖：

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713594614690.png)

2、同时，因fetch 函数是在浏览器环境中全局定义的，所以在浏览器环境中可以直接使用。但是，在 vite.config.ts 中使用 fetch 函数时，可能还未加载到浏览器环境中，所以需要特别处理才能在 vite.config.ts 中使用。需要使用 node-fetch 这个第三方模块将 fetch 函数兼容到 node.js 环境中，这样就可以在 vite.config.ts 中直接使用 fetch 函数：

![1713594625919](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713594625919.png)

3、在 vite.config.ts 文件中添加如下代码将 fetch 函数兼容到 node.js 环境：

```js
import fetch from 'node-fetch'
(global as any).fetch = fetch
```

4、在 vite.config.ts 中书写接口调用函数来获取内容：

```js
// 接口返回数据的类型
interface IHtmlHeadContent {
  seo: {
    title: string;
    description: string;
    keywords: string;
  };
}
async function getHtmlHeadContent(): Promise<IHtmlHeadContent> {
  let url = '';
  // 判断是否是生产环境
  if (process.env.NODE_ENV === 'development') {
    url = 'https://www.book-waves.com/dev/home/data.json';
  } else {
    url = 'https://www.book-waves.com/home/data.json';
  }
  const response = await fetch(url);
  const data = await response.json();
  return data as IHtmlHeadContent;
}
```

5、向html文件中注入：

```js
plugins: [
    react(),
    createHtmlPlugin({
      minify: true,
      /**
       * 需要注入 index.html ejs 模版的数据
       */
      inject: {
        data: {
          title: (await getHtmlHeadContent()).seo.title,
          description: (await getHtmlHeadContent()).seo.description,
          keywords: (await getHtmlHeadContent()).seo.keywords,
        },
      },
    }),
  ],
```

6、在html文件中获取到注入的内容：

```html
<title><%- title %></title>
<meta name="description" content="<%= description %>" />
<meta name="keywords" content="<%= keywords %>" />
```

