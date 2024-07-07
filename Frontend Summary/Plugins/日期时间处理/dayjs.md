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

解析日期，格式化日期，操作日期：

```js
const date = dayjs('2021-09-01');
const formattedDate = dayjs('2021-09-01').format('YYYY-MM-DD');
console.log(formattedDate); // 输出：2021-09-01
```

使用 isBefore() 方法判断时间是否小于今天：

```
const inputDate = '2021-09-01'; // 示例日期
const today = dayjs().startOf('day'); // 今天的开始
const isBeforeToday = dayjs(inputDate).isBefore(today);
console.log('Is the input date before today?', isBeforeToday);
```

使用 isAfter() 方法判断时间是否大于今天：

```

const isAfterToday = dayjs(inputDate).isAfter(today);
console.log('Is the input date after today?', isAfterToday);
```

要使用 Day.js 将时间设置为一天的开始或结束，你可以使用 startOf() 和 endOf() 方法。以下是如何将时间设置为一天的开始和结束的示例：使用 startOf() 方法将时间设置为一天的开始：

```
const now = dayjs(); // 当前时间
const startOfDay = now.startOf('day'); // 一天的开始
console.log('Start of Day:', startOfDay.format());
```

使用 endOf() 方法将时间设置为一天的结束：

```

const endOfDay = now.endOf('day'); // 一天的结束
console.log('End of Day:', endOfDay.format());
```

这些方法可以与其他时间单位一起使用，例如 'month'、'year' 等，以将时间设置为相应单位的开始或结束。例如，要将时间设置为一个月的开始和结束，可以使用 startOf('month') 和 endOf('month')。

Day.js 还提供了许多其他功能，如插件支持、本地化等。要了解更多关于 Day.js 的信息，请查阅官方文档：https://day.js.org/

## UTC

默认情况下，Day.js 会把时间解析成本地时间。它会根据用户计算机的系统时间和时区设置自动获取当前的本地时间。



如果想使用 UTC 时间，您可以调用 dayjs.utc() 而不是 dayjs()。



在 UTC 模式下，所有显示方法将会显示 UTC 时间而非本地时间。

- 
- 
- 
- 

```
// 默认是当地时间dayjs().format() //2019-03-06T08:00:00+08:00// UTC 时间dayjs.utc().format() // 2019-03-06T00:00:00Z
```



现在，你可以使用 Day.js 的 utc() 方法处理 UTC 时间，然后使用 tz() 方法将其转换为不同地区的时间。例如，将 UTC 时间转换为纽约时间：

- 
- 
- 

```
const utcTime = '2021-09-01T12:00:00Z'; // UTC 时间const newYorkTime = dayjs.utc(utcTime).tz('America/New_York').format();console.log('New York Time:', newYorkTime);
```



要将 UTC 时间转换为其他地区的时间，只需将 tz() 方法中的时区参数更改为所需的时区。例如，将 UTC 时间转换为东京时间：

- 
- 

```
const tokyoTime = dayjs.utc(utcTime).tz('Asia/Tokyo').format();console.log('Tokyo Time:', tokyoTime);
```



请注意，时区字符串（如 'America/New_York' 和 'Asia/Tokyo'）是基于 IANA 时区数据库的。你可以在这里找到完整的时区列表：https://en.wikipedia.org/wiki/List_of_tz_database_time_zones



在 Day.js 中，format() 函数用于格式化日期和时间。在格式字符串中，大写和小写字母表示不同的格式化选项。以下是一些常见的大写和小写字母格式选项及其含义：



年份：

YYYY：4 位数的年份，例如 2021。

YY：2 位数的年份，例如 21。



月份：

MM：2 位数的月份，例如 01、02 等。

MMM：月份的缩写名称，例如 Jan、Feb 等。

MMMM：月份的完整名称，例如 January、February 等。



日期：

DD：2 位数的日期，例如 01、02 等。

D：1 位数的日期，例如 1、2 等。



星期：

d：一周中的第几天，周日为 0，周一为 1，依此类推。

dd：星期几的缩写名称，例如 Su、Mo 等。

ddd：星期几的简写名称，例如 Sun、Mon 等。

dddd：星期几的完整名称，例如 Sunday、Monday 等。



小时：

HH：24 小时制的小时，例如 00、01、23 等。

hh：12 小时制的小时，例如 01、02、11 等。



分钟：

mm：2 位数的分钟，例如 00、01、59 等。



秒：

ss：2 位数的秒，例如 00、01、59 等。



上午/下午：

A：大写的上午/下午标识符，例如 AM、PM。

a：小写的上午/下午标识符，例如 am、pm。



这些仅是 Day.js 中可用的一些格式选项。更多格式选项和详细信息，请参阅 Day.js 文档：https://day.js.org/docs/en/display/format

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

