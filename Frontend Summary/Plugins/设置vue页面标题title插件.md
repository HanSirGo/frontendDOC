### 路由守卫
#### 1. 每个页面设置相同标题
index.html
直接修改title标签里面的标题
(ps: 这个html文件中也可以引入一些js文件)
#### 2. 每个页面设置不同标题
router - index.js

```javascript
const router = new Router({
    mode: 'history',
    routes: [
        {
            path: '/index',
            name: 'index',
            component: Index,
            meta:{
                // 页面标题title
                title: '首页'
            }
        },
        {
            path: '/content',
            name: 'content',
            component: Content,
            meta:{
                title: '内容'
            }
        }
    ]
})
export default router
```

main.js
```javascript
import router from './router'
router.beforeEach((to, from, next) => {
  /* 路由发生变化修改页面title */
  if (to.meta.title) {
    document.title = to.meta.title
  }
  next()
})
```
### 插件 vue-wechat-title
#### 下载
```javascript
npm install vue-wechat-title --save
```
#### 使用
1. main.js

```javascript
import VueWechatTitle from 'vue-wechat-title';

Vue.use(VueWechatTitle);
```
2. router文件

```javascript
{

        path: '/connect',

        name: '个人中心',

        component: resolve => { require(['@/components/connect/connect'], resolve) },

        meta: { title: "个人中心" }

}
```
3. APP.vue下的router-view添加配置
```javascript
<router-view v-wechat-title="$route.meta.title" />
```