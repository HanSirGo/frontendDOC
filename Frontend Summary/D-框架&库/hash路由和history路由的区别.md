## hash路由和history路由的区别

### history和hash路由的区别

hash路由和history路由在Web开发中都是非常常见的两种路由方式，它们在多个方面都存在明显的区别。以下是对它们之间主要区别的详细解释：

1. URL表现：

- hash路由：在URL中，hash路由会携带一个“#”号，所有的路由变化都会在URL前面添加“#/”符号。例如，`http://example.com/#/user/123`。
- history路由：与hash路由不同，history路由的URL中不包含“#”号，它使用正常的路径形式，更符合用户的直观感受，URL更加美观。例如，`http://example.com/user/123`。

2. 实现原理：

- hash路由：基于浏览器的hashchange事件来监听URL中hash值的变化，从而实现页面的跳转。在hash模式下，路由的机制是使用window.location中的hash属性，将路由路径添加到URL的hash值中，然后在JavaScript中通过监听hash值的变化来响应路由变化。hash路由的优点之一是它不会将hash路径发送到服务器，因此可以避免服务器配置的问题。
- history路由：基于HTML5 History API提供的history.pushState方法或history.replaceState方法，以及popstate事件来实现。与hash路由不同，history路由需要客户端和服务端共同的支持。当使用history模式时，服务器需要进行相应的配置，以确保在直接访问URL时能够返回正确的页面。

3. 刷新页面和重新请求：

- hash路由：在hash模式下，即使页面刷新，浏览器仍然只会请求页面的初始HTML文件，所有的路由变化都在客户端进行处理，不会重新发送HTTP请求到服务器。
- history路由：在history模式下，当URL发生变化时（例如，用户点击了浏览器的后退或前进按钮），浏览器会重新请求服务器以获取对应的页面内容。这意味着使用history路由时，需要确保服务器能够处理这些请求并返回正确的页面。

4. 兼容性和配置：

- hash路由：hash路由的兼容性较好，它支持所有现代浏览器，并且在不支持HTML5 History API的旧版浏览器上也能正常工作。在Vue路由中，hash路由是默认模式，不需要额外的配置。
- history路由：相对于hash路由，history路由需要更复杂的配置和服务器支持。使用history模式时，需要确保服务器能够处理所有可能的URL路径，并在直接访问这些URL时返回正确的页面。

### 部署出现404是怎么回事

在使用Vue Router的history模式进行项目打包部署时，出现404错误的原因通常与后端服务器的配置有关。在history模式下，Vue Router通过修改浏览器的历史记录来实现路由切换，而不是依赖于URL中的hash部分。但是，当用户直接访问某个URL（如通过书签、链接或刷新页面）时，如果服务器端没有相应的处理逻辑，就会返回404错误。

#### 原因

当使用 history 模式时，服务器需要能够处理所有可能的 URL 路径，并返回应用的入口文件（通常是 index.html）。这是因为当用户直接访问一个路由（如通过书签、链接或刷新页面）时，浏览器会向服务器发送一个请求来获取该路径下的内容。如果服务器没有正确配置，它将无法识别这个路径，并返回一个 404 错误。

Hash模式通过URL的hash部分（#及其后面的部分）来模拟一个完整的URL，但实际上这部分在HTTP请求中并不会被发送到服务器。因此，无论用户访问哪个hash路由，服务器都只会收到对初始页面的请求，并返回该页面。服务器不需要为每一个hash路由配置特定的处理逻辑。

#### 解决

为了解决这个问题，你需要在后端服务器上正确配置以处理所有可能的路由请求。以下是一些可能的解决方案：

1. **配置后端服务器**：在后端服务器上设置一个通配符路由，将所有非文件的请求都重定向到你的应用的入口页面（通常是index.html）。这样，当用户刷新页面时，服务器将返回应用的入口页面，然后由Vue Router接管路由的处理。具体的配置方式取决于你使用的后端服务器软件，例如Node.js + Express、Nginx等。

对于Node.js + Express，你可以在路由配置中添加一个通配符路由，如下所示：

```js

app.get('*', (req, res) => {
  res.sendFile(path.resolve(__dirname, 'dist/index.html'));
});
```

对于Nginx，你可以在配置文件中添加类似以下的配置：

```js
server {
  listen  80;
  server_name  www.xxx.com;

  location / {
    index  /data/dist/index.html;
    try_files $uri $uri/ /index.html;
  }
}
```

2. **确保Vue Router的配置正确**：在Vue Router中，确保你使用了正确的模式（即'history'），并且你的路由配置是正确的。此外，如果你使用了Vue CLI进行项目构建，你可能需要在`vue.config.js`文件中设置`publicPath`选项，以确保静态资源的正确加载。

