## Day.js

> Day.js 是一个仅 2kb 大小的轻量级 JavaScript 时间日期处理库，下载、解析和执行的JavaScript更少，为代码留下更多的时间。

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

### 安装

```js
npm i dayjs

import dayjs from 'dayjs'
const dayjs = require('dayjs')
```

### 基础使用

```js
# 当前时间
dayjs()
dayjs('2024-05-29') // 字符串
dayjs(new Date(2024,5,29)) // Date对象
dayjs(1318781876406) // number 时间戳
```

##### 自定义事件格式

```js
# 解析自定义时间格式 如：
dayjs('12-25-2024', 'MM-DD-YYYY') // 可以使用插件 CustomParseFormat
```



### UTC和本地时间互相转换

```js
// 默认情况下，Day.js 会把时间解析成本地时间。
// 如果想使用 UTC 时间，您可以调用 dayjs.utc() 而不是 dayjs()。
// 在 UTC 模式下，所有显示方法将会显示 UTC 时间而非本地时间。
// 这依赖 UTC 插件，才能正常运行
import utc from 'dayjs/plugin/utc'
dayjs.extend(utc)

// 默认是当地时间
dayjs().format() //2019-03-06T08:00:00+08:00
// UTC 时间
dayjs.utc().format() // 2019-03-06T00:00:00Z
```

#### 本地时间转换为UTC

```js
dayjs().utc().format()
// 获取UTC时间'2023-07-09T01:22:22Z'

dayjs.utc().format()
// 获取UTC时间'2023-07-09T01:22:22Z'

// 本地时间是02:00:00，时区是8，所以UTC是昨天18:00:00
dayjs('2023-07-09 02:00:00').utc().format('YYYY-MM-DD HH:mm:ss')
// '2023-07-08 18:00:00' 

// UTC转换为UTC时间
dayjs.utc('2023-07-09 02:00:00').format('YYYY-MM-DD HH:mm:ss')
// '2023-07-09 02:00:00'
```

#### UTC转换为本地时间

```js
// 假设UTC时间1688896817000

// UTC转换本地时间
dayjs(1688896817000).format('YYYY-MM-DD HH:mm:ss')
// '2023-07-09 18:00:17'
// 毫秒数默认认为是本地，本地比UTC快，所以转换本地的就快

// UTC转换为UTC时间
dayjs.utc(1688896817000).format('YYYY-MM-DD HH:mm:ss')
// '2023-07-09 10:00:17'
```

```js
// 假设我们有一个UTC时间
const utcTime = '2023-04-01T12:00:00Z' // 格式必须是ISO 8601
// 使用 dayjs设置utc时间
cosnt utc = dayjs.utc(utcTime)
// 转为北京时间
const bjTime = utc.utcOffset(480).format('YYYY-MM-DD HH:mm:ss') // 480分钟代表8小时

// 获取ISO 8601 格式的时间
utc.format() //默认情况下，dayjs使用 ISO 8601格式

# 例子
const dayjs = require('dayjs')
const utc = require('dayjs/plugin/utc')
dayjs.extend(utc)
// utc 时间
console.log(dayjs.utc('2024-05-11 17:08:00').format())
// utc 转 北京时间
console.log(dayjs.utc('2024-05-11 17:08:00').utcOffset(480).format('YYYY-MM-DD HH:mm:ss'))
// 北京时间 转 utc
console.log(dayjs('2024-05-11 17:08:00').utc().format())

2024-05-11T17:08:00Z
2024-05-12 01:08:00
2024-05-11T09:08:00Z
```

```js
# 设置UTC为当天结束日时间
// 正确用法
dayjs().utc().endOf('day').format('YYYY-MM-DD HH:mm:ss')
'2023-07-09 23:59:59'

// 错误用法
dayjs().endOf('day').utc().format('YYYY-MM-DD HH:mm:ss')
'2023-07-09 15:59:59'
```

