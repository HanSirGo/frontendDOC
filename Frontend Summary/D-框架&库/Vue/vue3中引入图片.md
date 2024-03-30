## 在Vue 3中加载本地图片和其他静态资源

在Vue 3中加载本地图片和其他静态资源有几种方法：

### 使用绝对路径：

将图片放在`public`目录下，然后使用绝对路径来引用它们。

例如，如果图片位于`public/images/logo.png`，可以在组件中使用`<img src="/images/logo.png">`来加载它。

**但是请注意：public目录下，图片不经过编译。**

### 使用相对路径：

如果图片和组件位于同一目录下，可以使用相对路径来引用它们。

例如，如果图片位于与组件相同的目录下，可以使用`<img src="./logo.png">`来加载它。

### 使用require导入：

Vue 3中，你可以使用`require`函数将图片作为模块导入，然后在组件中使用它。

首先，确保你已经安装了file-loader或url-loader插件。然后，在组件中使用以下代码：

```vue
<template>
  <div>
    <img :src="imageSrc" alt="My Image">
  </div>
</template>

<script>
import myImage from '@/assets/logo.png'; // 替换成你的图片路径

export default {
  data() {
    return {
      imageSrc: ''
    };
  },
  mounted() {
    this.imageSrc = require(`@/assets/logo.png`);
  }
};
</script>
```

### 使用import导入：

如果你使用了Webpack等构建工具，也可以使用ES6的`import`语法来导入图片。

首先，确保你已经安装了file-loader或url-loader插件。然后，在组件中使用以下代码：

```vue
<template>
  <div>
    <img :src="imageSrc" alt="My Image">
  </div>
</template>

<script>
import myImage from '@/assets/logo.png'; // 替换成你的图片路径

export default {
  data() {
    return {
      imageSrc: ''
    };
  },
  mounted() {
    import(`@/assets/logo.png`).then((src) => {
        this.imageSrc = src.default;
    });
  }
};
</script>
```

以上是几种加载本地图片和其他静态资源的方法，根据你的具体需求和项目配置选择适合的方法即可。