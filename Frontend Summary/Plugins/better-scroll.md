# vue如何使用better-scroll

![1723977086789](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723977086789.png)

## 1. vue如何使用better-scroll

better-scroll 是一个基于原生滚动的移动端滑动库，它可以让你非常简单地实现各种滑动效果，如列表滚动、轮播图等。

在 Vue 项目中使用 better-scroll 可以提高移动端页面的滑动性能和用户体验。

### 1.1. Vue 项目中集成 better-scroll 的基本步骤：

#### 1.1.1. **安装 better-scroll**:

首先，你需要通过 npm 或 yarn 安装 better-scroll。在你的项目目录下运行以下命令：

```
npm install @better-scroll/core --save
```

如果你需要额外的插件（比如 PullUpLoad、PullDownRefresh 等），你还需要单独安装它们：

```
npm install @better-scroll/pull-up @better-scroll/pull-down --save
```

#### 1.1.2. **导入并使用 better-scroll**:

在需要使用 better-scroll 的 Vue 组件中，导入核心库和任何必要的插件。例如：

```
import BScroll from '@better-scroll/core';
import PullUp from '@better-scroll/pull-up';
import PullDown from '@better-scroll/pull-down';

BScroll.use(PullUp);
BScroll.use(PullDown);
```

#### 1.1.3. **创建 better-scroll 实例**:

在组件的生命周期钩子（如 `mounted`）中初始化 better-scroll 实例，并将其绑定到你的 DOM 元素上：

```
export default {
    name: 'MyComponent',
    mounted() {
    this.$nextTick(() => {
        this.scroll = new BScroll('.scroll-container', {
        pullUpLoad: true, // 开启上拉加载
        pullDownRefresh: true, // 开启下拉刷新
        click: true, // 允许点击事件穿透
        });
    });
    },
};
```

#### 1.1.4. **监听滚动事件**:

你可以监听 `better-scroll` 的各种事件，

例如 `scroll`、`pullingUp` 和 `pullingDown`：

```
mounted() {
    this.$nextTick(() => {
    this.scroll = new BScroll('.scroll-container', {
        ...
    });

    this.scroll.on('pullingUp', () => {
        // 当用户上拉加载时触发
        this.loadMoreData();
    });

    this.scroll.on('pullingDown', () => {
        // 当用户下拉刷新时触发
        this.refreshData();
    });
    });
}
```

#### 1.1.5. **更新数据和重置 better-scroll**:

当你的数据发生变化时，需要调用 `refresh` 方法来重新计算布局：

```
methods: {
    loadMoreData() {
    // 加载更多数据...
    this.scroll.finishPullUp(); // 结束上拉加载状态
    },

    refreshData() {
    // 刷新数据...
    this.scroll.finishPullDown(); // 结束下拉刷新状态
    this.scroll.refresh(); // 重新计算布局
    },
},
```

以上就是在 Vue.js 中使用 better-scroll 的基本流程。

根据你的需求，你可能需要调整配置选项或添加更多的插件。

记得在销毁组件时清除 better-scroll 实例，以避免内存泄漏。

