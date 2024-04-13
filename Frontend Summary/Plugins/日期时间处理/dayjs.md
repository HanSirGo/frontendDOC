## Day.js

Day.js是一个极简的JavaScript库，可以为现代浏览器解析、验证、操作和显示日期和时间。其具有以下特点：

- 和 Moment.js 相同的 API 和用法
- 不可变数据 (Immutable)
- 支持链式操作 (Chainable)
- 国际化 I18n
- 仅 2kb 大小的微型库
- 全浏览器兼容

```js
dayjs().format();                                     // 2020-09-08T13:42:32+08:00
dayjs().format('YYYY-MM-DD');                         // 2020-09-08
dayjs().format('YYYY-MM-DD HH:mm:ss');                // 2020-09-08 13:47:12
dayjs(1318781876406).format('YYYY-MM-DD HH:mm:ss');   // 2011-10-17 00
```

**Github：**https://github.com/iamkun/dayjs/

