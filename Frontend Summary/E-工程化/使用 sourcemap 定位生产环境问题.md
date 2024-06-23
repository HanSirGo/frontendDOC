# 使用 sourcemap 定位生产环境问题（2种方案）

# **前言**

学会调试线上代码是每个前端都必须学会的技能，本篇将介绍使用两种方式调试线上环境中的代码。

```
先说说为什么要学会调试线上环境？
```

大部分情况下，遇到线上bug时，我们可以在本地启动，然后找到对应模块，复现出bug，最后进行修改。

但也有例外的情况，例如内外网连接的两个数据库内容不一样，导致接口返回的数据出现缺失；或者外网环境接口超时，无数据返回；因为环境不同导致的bug，在本地找到出错的地方是非常麻烦的。

# **1. sourcemap**

## **1. 什么是sourcemap？**

sourcemap是一个以 .map 为后缀的文件。它负责将编译、打包、压缩后的代码映射回源代码。

![1718452473325](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452473325.png)

通俗的来讲，sourcemap可以让你在生产环境中，找到你开发环境下代码报错的位置。

## **2. 如何生成sourcemap**

（1）webpack

```
devtool: "source-map",
```

devtool此选项控制是否生成，以及如何生成 source map。更多配置参考：**www.webpackjs.com/configurati…**[1]

对webpack配置不熟悉的，可参考：**webpack5详细教程（5.68.0版本）**[2]

（2）vite

```
build: {
  sourcemap: true,
},
```

更多配置如下所示：

![1718452503440](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452503440.png)

对vite不熟悉的，可参考：

1. **Vite5.0+Typescript+Vue3+Pinia 最新搭建企业级前端项目**[3]
2. **Vite5.0+Typescript+React18+Zustand 最新搭建企业级前端项目**[4]

## **3. 生成sourcemap示例**

这里以vite为例，先拉取**demo**[5]，然后配置好sourcemap: true，运行`yarn build`进行打包。

dist/static/js文件夹下，每个js文件，都有其相对应的.map文件

![1718452524717](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452524717.png)

最后一行注释的作用，是告知浏览器的开发者工具该JavaScript文件对应的源代码映射文件（Source Map文件）的位置

运行`yarn preview`进行本地预览，项目启动之后，选中开发者工具 -> More tools -> Developer resources

![1718452549967](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452549967.png)

可以从Developer resources中看到sourcemap文件已被加载，并在状态为success。

但这样一来，任何一个人都可以通过Source看到你源码

![1718452566259](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452566259.png)

这意味着公司项目的源码全部对外暴露，这肯定是不可接受的。这也是为什么webpack，vite在打包时，默认情况下是不会产生.map文件。

# **2. 手动添加sourcemap**

## **1. 配置**

思路：打包时生成.map文件，但不与原js文件自动进行关联。而是在需要时，手动去关联

webpack配置：

```
devtool: "hidden-source-map",
```

vite配置：

```
build: {  sourcemap: 'hidden',},
```

1. 依旧以上面使用vite搭建的demo项目为例，修改src/pages/user.tsx，特地写一个js错误（不熟悉React的，该代码段可直接拷贝，不影响接下来的阅读）

```js
import { useNavigate } from 'react-router-dom';

const obj: any = {
  b: 1,
};

const User = () => {
  const navigation = useNavigate();
  return (
    <div>
      {/* 报错：因为取不到c属性 */}
      {obj.b.c}
      user
      <button onClick={() => navigation('/manage')}>manage</button>
    </div>
  );
};

export default User;
```

2. 运行`yarn build`进行打包，可以看到最后一行注释已经没有了。

![1718452599584](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452599584.png)

3. 运行`yarn preview`进行本地预览

Developer resources中与项目源码相关的user-DGdhmubt.js.map文件并没有被加载

![1718452619996](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452619996.png)

并且浏览器控制台报错，报错信息如下所示：

![1718452632428](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452632428.png)

4. 点击第一行飘蓝的user-BQ5Z0b6n.js，跳转到Sources

![1718452653288](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452653288.png)

可以看到，这个经过编译之后的代码，与源码不一致，这时需借助sourcemap进行定位

5. 在浏览器中打开构建之后代码文件，然后右键，就可以看到 add source map的选项

![1718452673679](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452673679.png)

然后填上 soucemap 的路径就可以了。soucemap 的路径和 js 相同。例如我的user-BQ5Z0b6n.js路径为：**http://localhost:4173/static/js/user-BQ5Z0b6n.js，**[6] 在add source map中填入**http://localhost:4173/static/js/user-BQ5Z0b6n.js.map**[7]

6. 重新点击打印控制台第一行飘蓝的user.tsx，跳转到Sources，可以看到源码报错的地方。

![1718452696276](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452696276.png)

## **2. 优缺点**

优点：map文件可存放在本地，适用于生产环境不允许有sourcemap的项目。

例如生产环境的user-BQ5Z0b6n.js路径为**www.test.com/js/user-BQ5…**[8] ，在add source map中填入本地资源文件地址**http://localhost:4173/static/js/user-BQ5Z0b6n.js.map**[9]

缺点：1. 浏览器刷新之后需手动重新添加.map文件；2. 在实际项目中，因为路由懒加载等代码分割技术，会产生很多js文件，相应的也会产生多个map文件。在调试时需根据js文件名，手动找到对应的map文件并添加

# **3. 配置Nginx**

思路：在nginx上做拦截，只有对应的cookie才能访问.map文件

**1. 修改vite.config.ts**

```
build: {
  sourcemap: true,
},
```

**2. 修改index.html**

在`<script type="module" src="/src/main.tsx"></script>`下添加

```html
<script>
  document.cookie = 'your_cookie_name=your_cookie_value';
</script>
```

这里模拟添加cookie值

**3. 运行yarn build进行打包**

将打包后的dist放到Nginx目录，例如我的Nginx地址为D:\nginx-1.19.1

![1718452728205](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452728205.png)

**4. 修改config/nginx.conf**

添加

```js
server {
        listen       8080;
        server_name  localhost;

        location / {
            root   dist;
            index  index.html index.htm;
        }
        
        location ~* .map$ {
                if ($http_cookie !~ "your_cookie_name=your_cookie_value") {
                        return 403;
                }
                root   dist;
                try_files $uri =404;
        }

    }
```

location ~* .map$：匹配以.map结尾的文件

if ($http_cookie !~ "your_cookie_name=your_cookie_value")：检查请求头中的Cookie是否存在，并且其值是否包含指定的Cookie名称和值（your_cookie_name=your_cookie_value），不存在返回403状态码

**5. 启动Nginx**

双击nginx.exe启动Nginx，并访问**http://localhost:8080/**[10] ，点击打印控制台第一行飘蓝的user.tsx，跳转到Sources，可以看到源码报错的地方。配置成功

![1718452757265](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452757265.png)

对于没有设置Cookie的请求，例如这里删除index.html中的Cookie代码，然后重新打包部署（注意：先清空浏览器Cookie缓存）。Developer resources中可以看到.map文件加载失败，并且返回配置的403状态码

![1718452779056](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452779056.png)

这里只是简单模拟了该效果，在实际项目中，Cookie需根据账号权限进行配置。

# **总结**

线上调试有两种：1. 手动添加sourcemap；2. 配置Nginx，对没有权限的访问进行拦截。

第1种手动添加虽然麻烦了点，但无需后端配合。第2种调试方便，但需后端配合。具体使用哪种，视项目情况而定