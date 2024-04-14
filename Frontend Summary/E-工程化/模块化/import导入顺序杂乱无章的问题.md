# import导入顺序杂乱无章的问题

我们经常会遇到项目中的 `import` 语句顺序混乱的问题。这不仅会影响代码的可读性，还可能使我们代码在提交的时候产生不必要的冲突。

### 解决方案 eslint-plugin-import

开始我调研了一下 `eslint-plugin-import` 插件。这款插件的排序逻辑是这样：

1. builtin: 这代表Node.js内置的模块，例如fs、path等。

```js
import fs from 'fs';
```

2. external: 这代表外部库或框架，通常是你通过npm或yarn安装的模块。

```js
import axios from 'axios';
```

3. internal: 这代表项目内部的模块，但它们通常在项目的不同目录或子目录中。这些模块不是直接的父级或同级模块。

```js
import { someFunction } from '@utils/my-helper';
```

4. parent: 这代表从父目录导入的模块。

```js
import something from '../something';
```

5. sibling: 这代表与当前文件在同一目录下的其他文件。

```js
import { siblingFunction } from './sibling-module';
```

6. index: 这代表从目录的index文件导入的模块。

```js
import { indexFunction } from './';
```

7. object: 这代表导入的对象属性，例如：

```js
import('some-module').then(({ someExport }) => ...);
```

8. type: 这代表从模块导入的类型或接口（这在TypeScript中特别有用）。

```js
import type { MyType } from './types';
```

大致分为这些模块。我们可以在 `eslint` 的规则中根据这些模块进行排序。但是这并不是我想要的排序模式，我更**希望根据功能进行排序。组件放在一起，hooks 放在一起，工具函数放在一起**，等等。

### eslint-plugin-simple-import-sort

于是我找到了 `eslint-plugin-simple-import-sort` 这个插件。使用方式如下：

安装插件:

```js
npm install --save-dev eslint-plugin-simple-import-sort
```

配置 `ESLint`: 在 `.eslintrc.js` 或你的 `ESLint` 配置文件中，添加以下配置：

```js
module.exports = {
  // ... 其他配置
  plugins: ['simple-import-sort'],
  rules: {
    'simple-import-sort/imports': 'error',
    'simple-import-sort/exports': 'error',
  },
};
```

自定义排序:

```js
'simple-import-sort/imports': [
  'error',
  {
    groups: [
      [`^vue$`, `^vue-router$`, `^ant-design-vue$`, `^echarts$`], // 你可以根据需要添加更多的内置模块
        [`.*\\.vue$`], // .vue 文件
        [`.*/assets/.*`, `^@/assets$`],
        [`.*/config/.*`, `^@/config$`],
        [`.*/hooks/.*`, `^@/hooks$`],
        [`.*/plugins/.*`, `^@/plugins$`],
        [`.*/router/.*`, `^@/router$`],
        [`^@/services$`, `^@/services/.*`],
        [`.*/store/.*`, `^@/store$`],
        [`.*/utils/.*`, `^@/utils$`],
        [`^`],
        [`^type `],
    ],
  },
],
```

这样就会按照我们系统中的功能模块排序了！