## 取而代之 new Date()

### **Date背景**

------

众所周知，在 1995 年，**Brendan**（JavaScript之父） 被网景公司安排了一个巨大而紧急的工作任务，用 10 天的时间来编写 JavaScript 语言。而 `日期处理` 是几乎所有编程语言的基本部分，所以JavaScript 也必须拥有它。

这是一个非常复杂的领域，但留给作者编写它的时间却很短。最终 **Brendan** 选择了借鉴当时红极一时的 `java` 语言，从`java.Util.Date`日期实现中复制了 Javascript 的日期对象。坦率地说，这个实现很糟糕。事实上 `Java` 在两年后的 1.1 版本中就弃用和替换这种实现。然而 20 年后，我们仍然在 JavaScript 编程语言中使用这个 API。

#### Date存在的问题

------

- **不支持除用户本地时间以外的时区**

不支持开发人员通过 **API** 来切换`时区信息`。

- **解析器行为不可靠以至于无法使用**

```js
new Date(); 
new Date(value); 
new Date(dateString); 
new Date(year, monthIndex [, day [, hours [, minutes [, seconds [, milliseconds]]]]]);
```

开发人员常常因为`输入的参数格式问题`，引发时间错误，导致程序崩溃。比如输入 `('2022-02-22')` 和 `(2022,02,22)` 得到的结果却不同，

- **计算 API 缺失**

涉及时间的运算逻辑通常都需要开发人员自己去写，比如`比较两个时间的长短`，`时间之间的加减运算`，没有自己的计算API

- **不支持非公历**

除了全球通用的`公历`外，无法使用各国的自己的历法。比如中国的`农历`

#### Temporal的诞生

------

为了弥补 `Date` 的缺陷，很多程序员着手开发一些开源的库来绕过对 `Date` 的直接使用，比较优秀的npm库如 date.js 、moment.js，但 Date 的问题始终困扰着 **Javascript** 这门语言的进一步发展，于是 **TC39** 组织开始了对 `Date` 的升级改造，他们找到了 moment.js 库的作者，Maggie ，由她来担任新特性 `Temporal`的主力设计。

*感兴趣的同学可以去博客[1]阅读更多细节，该网页的控制台已经支持Temporal 对象或者在本地运行，安装Temporal的  **polyfill***

```js
$ npm install @js-temporal/polyfill
import { Temporal} from '@js-temporal/polyfill';
```

`Temporal`是一个全局对象，像 `Math` 、`Promise` 一样位于顶级命名空间中，为 **Javascript** 语言带来了现代化的日期、时间接口。

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714207150150.png)

如图所示，一个全面的`Temporal`包含三个部分：

绿色区域为 `ISO 8601` 格式的 `日期和时间`；

黄色区域为`时区`（日本东京）；

红色区域为`日历`（日本历法）；

> `ISO 8601格式`：国际通用时间格式，T用来分割日期（2020-08-05）和时间（20:06:13），“+”或者“-”分别代表东时区和西时区。+09:00，代表东九区。

**对比 Date**

```js
new Date()
//Fri Jan 28 2022 17:03:11 GMT+0800 (中国标准时间)
```

`Date` 采用 GMT格式（旧的时间表示格式） 的时间，使用方面不如 ISO 8601 通用，同时不包含 `时区和历法`。

#### Temporal各种类型介绍

------

推翻重新设计的`Temporal`，包含5种主要 `类型`，每个类型负责不同的功能，类型之间还可以相互进行`转换`。

学会了这5种类型的 `功能` 以及类型的之间的 `关系` ，就基本掌握了`Temporal`。

下面是`Temporal`各种类型的功能与转换关系图，非常重要，十分有利于我们全面理解和使用`Temporal`，下文将逐步讲解。

![1714207172016](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714207172016.png)

> **ZonedDateTime**

**定义**：最全面的`Temporal`类型，与时区和日历都有关联。表示从地球上`特定区域`的角度来看，在`特定时刻`发生的事。

**使用场景**：在`北京时间`的 `2008年5月12日14时28分4秒`发生汶川大地震，或者 在 `纽约时间`的 `2008年5月12日01时28分4秒`发生了汶川大地震。

##### 如何获得一个 ZonedDateTime 类型？

------

不仅是获得一个 ZonedDateTime 类型，其实所有的  `Temporal` 类型都是一样的获取途径。通常有两种方法获得，分别是 `new 构造函数（）`，和`from方法`。

- **new 构造函数（）方式**

参数：（纳秒数，时区，日历），**不同类型要求的参数不同**。

纳秒数：从 `Unix 纪元`（1970 年 1 月 1 日午夜 UTC）计算，所经过的纳秒数，单位为**bigint**

时区，日期：可以是字符串，也可以是 `Temporal` 类型。

```js
new Temporal.ZonedDateTime(0n, 'Asia/Shanghai', 'chinese'); 
//Temporal.ZonedDateTime <1970-01-01T08:00:00+08:00[Asia/Shanghai][u-ca=chinese]>
```

通常每个`Temporal`类型的都有`toString()`方法，覆盖了`Object.prototype.toString()`方法，作用是通过一个字符串表示`Temporal`

调用 `toString()` 用字符串来表达，方便阅读。

```js
new Temporal.ZonedDateTime(0n, 'Asia/Shanghai', 'chinese').toString(); 
//1970-01-01T08:00:00+08:00[Asia/Shanghai][u-ca=chinese]
```

这个 **ZonedDateTime** 的类型含义为，从北京时间看，**unix纪元** 起始时间为 `1970-01-01T08:00:00+08:00`，而非`1970-01-01T00:00:00+00:00`

- **from（）形式获得一个** `Temporal` **类型**

from（）的参数更为多样，同时支持 `溢出处理` (下文有讲解)，所以通常作为首选方法。通常作为获得一个 `Temporal` 类型的首选方法。

接受字符串

```js
Temporal.ZonedDateTime.from('2022-02-28T00:00:00+08:00[Asia/Shanghai]').toString(); 
//2022-02-28T00:00:00+08:00[Asia/Shanghai]
```

or

接受对象，（{时区，日期，日历}，options）

**options** 代表 `容错机制` 配置，即可以处理输入的`日期溢出`问题，有两种配置选项，`{ overflow: 'constrain' }`：自动处理溢出。`{ overflow: 'reject' }`，日期溢出则报错。

比如下面例子中，2022年2月一共28天，如果选择了 `constrain` 配置，输入日期超过了会进行`溢出处理`，即匹配最接近的存在值。

```js
//输入 31 天，得到 28 天
Temporal.ZonedDateTime.from({ timeZone: 'Asia/Shanghai', year: 2022, month: 2, day: 31}, { overflow: 'constrain' }).toString(); 
//2022-02-28T00:00:00+08:00[Asia/Shanghai]
```

选择 `reject` 配置，日期超出则报错。

```js
Temporal.ZonedDateTime.from({ timeZone: 'Asia/Shanghai', year: 2022, month: 2, day: 31}, { overflow: 'reject' }).toString(); 
//RangeError: value out of range: 1 <= 31 <= 28
```

> **Instant**

**定义**：负责单个时间点（称为 **“精确时间”** ），精度以纳秒为单位。不存在时区和日历信息。

**使用场景**：`2020-01-23T17:04:36.491865121-08:00`，只用来表达一个`瞬间`的时间，没有其他意义。

##### 获得一个 `Instant` 类型

------

**new Temporal.Instant( bigint )**

**bigint**：纳秒数，从 `Unix 纪元`（1970 年 1 月 1 日午夜 UTC）计算，所经过的纳秒数，单位为bigint

```js
new Temporal.Instant(1553906700000000000n);
//2019-03-30T00:45:00Z
new Temporal.Instant(0n);
//1970-01-01T00:00:00Z
new Temporal.Instant(-2208988800000000000n);
//1900-01-01T00:00:00Z
```

**Z** 在 `ISO 8601` 时间格式表示，没有时区关联。

**Temporal.Instant.from(thing: any)**

from方法在生成 `Instant` 时，会考虑时区的偏差。

```js
Temporal.Instant.from('2019-03-30T01:45:00+01:00[Europe/Berlin]');  
 Temporal.Instant.from('2019-03-30T01:45+01:00');
 Temporal.Instant.from('2019-03-30T00:45Z');
```

虽然前两个携带了时区信息，但获取到的Instant 时间值相同，三个都是 **2019-03-30T00:45Z**

> **Plain XX系列**

负责 `Temporal` 的 **日历日期** (xx年xx月xx日) 和 **钟表时间** （xx点xx分xx秒）表达，不涉及时区，

**使用场景**：日历日期：小红的生日是`农历` 每年3月25。钟表时间：现在是`下午2：00`

对比 `Instant`，双方的 **使用场景** 不同， **内部的属性** 也不同，`Instant` 不包含时区和日期，`Plain XX` 系列则包含日历。

Plain XX系列包含 5种 类型，覆盖最广的 `PlainDateTime` 包含日期和时间，还有只包含日期的 `Plaindate` 和只包含时间的 `Plaintime`。日期类型里，还有分类更精细的`PlainYearMonth`（年月）和 `PlainMonthDay`（月天）

以 `PlainDateTime` 举例，其他的类同。

##### 获取一个 `PlainDateTime`

------

**new Temporal.PlainDateTime（year,month,day...）**

参数以 `年 -> 纳秒` 顺序排列，其中 年 月 日，为必填项，其余选填。

```js
new Temporal.PlainDateTime(2020, 3, 14, 13, 37)
//2020-03-14T13:37:00
```

**Temporal.PlainDateTime.from()**

```js
Temporal.PlainDateTime.from({ year: 2001, month: 1, day: 1, hour: 25 ,calendar:'chinese'}, { overflow: 'constrain' }).toString()
//2001-01-24T23:00:00[u-ca=chinese]
```

> **TimeZone**

**定义**：负责 `Temporal` 时区的相关信息。

**例子**：北京时区，`东八区`，**不单独使用**，通常结合其他类型搭配。

##### 获取一个 `TimeZone` 类型

------

**new Temporal.TimeZone（string）**

**string**：对一个时区的描述

```js
//东八区，即北京时间
new Temporal.TimeZone('8:00');
//直接字符串描述，前提是 Temporal 内部有定义
new Temporal.TimeZone('Asia/Shanghai');
//Asia/Shanghai
```

from同理

```js
Temporal.TimeZone.from('Asia/Shanghai');
//Asia/Shanghai
```

在和其他类型搭配时，可以直接使用字符串(“Asia/Shanghai”),或者 Temporal.TimeZone对象

例子：

获取一个 `ZonedDateTime` 类型，设置时区时，使用 `Temporal.TimeZone` 对象。

```js
new Temporal.ZonedDateTime(0n, Temporal.TimeZone.from('Asia/Shanghai')); 
//1970-01-01T08:00:00+08:00[Asia/Shanghai]
等价于
new Temporal.ZonedDateTime(0n, 'Asia/Shanghai')); 
```

> **Calendar**

定义：负责 `Temporal` 的日历系统。

例子：中国`农历`。不单独使用，结合其他类型搭配。

##### 获取一个 `Calendar` 类型

------

同 `TimeZone` 类同，**new Calendar（string）** 或者 **Temporal.Calendar.from(string)**

```js
new Temporal.Calendar('chinese').toString();
//chinese
Temporal.Calendar.from('chinese').toString();
//chinese
```

`Calendar` 类型不会单独使用，要配合其他带有 `日历属性` 的类型使用。

如上所述，在 Temporal 里，包含 `日历属性` 的有 `plainXX系列` 和 `ZonedDateTime`

这两种类型的原型上有一个 `withCalendar`的方法，用来设置该日期的日历属性。

例子：

**plainXX系列 添加日历属性**

```
没有添加日历属性前
Temporal.PlainDate.from('2019-02-06');
//2019-02-06
添加日历属性后
Temporal.PlainDate.from('2019-02-06').withCalendar('chinese');
//2019-02-06[u-ca=chinese]
```

**ZonedDateTime 添加日历属性**

```js
没有添加日历属性前
Temporal.ZonedDateTime.from('2022-02-28T00:00:00+08:00[Asia/Shanghai]')
//2022-02-28T00:00:00+08:00[Asia/Shanghai]
添加日历属性后
Temporal.ZonedDateTime.from('2022-02-28T00:00:00+08:00[Asia/Shanghai]').withCalendar('chinese')
//2022-02-28T00:00:00+08:00[Asia/Shanghai][u-ca=chinese]
```

> **Duration**

**定义**：表示一段持续时间，并且这段时间可以用来进行 `算术`。

**使用场景**：两段时间，`一小时一分钟` 和 `一小时十分钟` ，可以转换 `Duration` 类型,再进行时间的长度比较，从而得知前者的时长 小于 后者。

`Duration` 并非像 `Date` 的 **时间戳** 形式那样表达一段时间，而是根据 `ISO 8601` 表示法生成一个 **字符串** 来表达一段时间。

简而言之，`ISO 8601` 表示法的首字母 **必须** 由 `P` 开头，后跟日期，年、月、周和日 再由 `T` 字母进行分割，后跟时间，小时、分钟、 秒。

一个 `Duration` 字符串可以缺失 **年 / 月 / 周 / 日 / 小时 / 分 / 秒** 中的任意一个，但必须包含首字母 `P`，如果同时有 **小时 / 分 / 秒**，则必须包含字母 `T`.

举例：

**一年**: `P1Y`，必须保留 `P` ，没有时间信息，不用加 `T`来分割。

**一分钟**: `PT1M`，必须保留 `P` ，有时间信息，则加 `T`来分割日期和时间

> 一些`Duration` 的字符串表达练习

![1714207331213](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714207331213.png)

##### 获得一个 `Duration` 类型

------

**new Temporal.Duration（）**

参数：年 => 纳秒，全部可选，非必填。需要按照顺序输入，某单位空缺则输入 `undefined` 或者 `0`。

```js
new Temporal.Duration(1, 2, 3, 4, 5, 6, 7, 987, 654, 321); 
// P1Y2M3W4DT5H6M7.987654321S  
//中文翻译 => 1年2月3周4天5小时6分钟7秒987毫秒654微秒321纳秒 
new Temporal.Duration(0, 0, 0, 40); 
// P40D  中文翻译 => 40天
Temporal.Duration.from(undefined, undefined, undefined, 40); 
// P40D 
new Temporal.Duration(); 
// PT0S 
```

了解了 `Duration`的字符串含义以及怎么生成一个 `Duration`后，可以用其进行一些 `日期与时间` 的计算与运算。

> 比对 `日期或时间` 的长度大小

调用 `Duration` 原型上的 `compare` 方法。返回值：-1， 0， 1

```js
one = Temporal.Duration.from({ hours: 79, minutes: 10 });//PT1H10M
two = Temporal.Duration.from({ days: 3, hours: 7, seconds: 630 });//P3DT7H630S
Temporal.Duration.compare(one,two)
//-1
```

返回-1，则 `one` 比 `two` 的时间短

返回0，则 `one` 比 `two` 的时间一样

返回-1，则 `one` 比 `two` 的时间长

事实上，除了 `Timezone` 和 `Calendar` 类型外，所有具备日期和时间属性的类型都可以进行`算术`

如 `PlainDateTime` 类型：

```js
one = Temporal.PlainDateTime.from('1995-12-07T03:24');
two = Temporal.PlainDateTime.from('1995-12-07T01:24');
Temporal.PlainDateTime.compare(two,two)
//1
```

> `日期或时间`的 加减运算。

**加法：**

```js
Temporal.Duration.from('PT1H'); //PT1H
hour.add({ minutes: 30 }); 
// => PT1H30M
```

**减法：**

```js
hourAndAHalf = Temporal.Duration.from('PT1H30M'); //PT1H30M
hourAndAHalf.subtract({ hours: 1 }); // => PT30M
```

同样，这些算术除了除了 `Timezone` 和 `Calendar` 类型外，其他类型都适用。

如 `PlainDateTime` 类型：

```js
dt = Temporal.PlainDateTime.from('1995-12-07T03:24:30.000003500'); 
dt.add({ years: 20, months: 4, nanoseconds: 500 }); 
// => 2016-04-07T03:24:30.000004
```

#### Temporal类型之间的转换

Temporal的各种类型，除了完成自身的功能外，还可以 `类型转换`。

![1714207386347](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714207386347.png)

再次回看这个类型关系图，左侧黄色区域的 **Instant** 类型，用来表达某个瞬间的时间，不包含时区和日历的信息。

右侧黄色区域的 **PlainXX系列**（5个），用来表达日历日期或者钟表时间，包含日历信息，而中间的 **ZonedDateTime** 则横跨左右两个区域，包含时区和日历信息，可以作为一个`通道`，连接左侧的 **Instant** 和右侧的 **Plain系列**，负责类型之间转换的`桥梁`，同时中间的 **Timezone** 时区类型 **Calendar** 日历类型，不单独使用，配合上方的 **ZonedDateTime** 类型来辅助转换。

最下面的 **Duration** 与所有类型没有直接关系，不参与类型转换，表示一段持续时间，并且这段时间可以用来进行算术。

**Instant  =>  ZonedTimeDate**

```js
转换前
Temporal.Instant.from('2020-08-05T20:06:13+0900').toString()
//2020-08-05T11:06:13Z
转换后
Temporal.Instant.from('2020-08-05T20:06:13+0900').toZonedDateTimeISO('Asia/Tokyo').toString();
//2020-08-05T20:06:13+09:00[Asia/Tokyo]
```

**ZonedTimeDate => Instant**

```js
转换前
Temporal.ZonedDateTime.from('2020-11-01T01:45-07:00[America/Los_Angeles]').toString();
//2020-11-01T01:45:00-07:00[America/Los_Angeles]
转换后
Temporal.ZonedDateTime.from('2020-11-01T01:45-07:00[America/Los_Angeles]').toInstant().toString();
//2020-11-01T08:45:00Z
```

**ZonedTimeDate  => PlainDateTime**

```js
转换前
Temporal.ZonedDateTime.from('2020-11-01T01:45-07:00[America/Los_Angeles]').toString()
//2020-11-01T01:45:00-07:00[America/Los_Angeles]
转换后
Temporal.ZonedDateTime.from('2020-11-01T01:45-07:00[America/Los_Angeles]').toPlainDateTime().toString();
//2020-11-01T01:45:00
```

**PlainDateTime =>  ZonedTimeDate**

```js
转换前
Temporal.PlainDateTime.from('2020-08-05T20:06:13').toString()
//2020-08-05T20:06:13
转换后
Temporal.PlainDateTime.from('2020-08-05T20:06:13').toZonedDateTime('Asia/Tokyo').toString();
//2020-08-05T20:06:13+09:00[Asia/Tokyo]
```

#### 总结

------

**回到最开始 Date 的问题。**

1.不支持除用户本地时间以外的时区。`Temparal` 支持开发人员通过 `TimeZone` 来设置本地时间以外的时区。

2.计算 API 缺失。除了时区和日历类型外，其他类型都可以进行 `算术运算`,即时间的比较，增加，减少等。

3.不支持非公历，`Calendar` 类型支持 `Temparal` 选择日历。

4.解析器行为不可靠以至于无法使用，在 `Temporal` 里，`new 构造函数（）` 或者`From` 方法，对参数的要求都更加规范，同时`From` 方法支持 `日期溢出` 后的逻辑处理，可以防止系统崩溃。

最后附一张 `Temparal` 各种类型的`功能`对照图。每个类型负责 `Temparal` 哪些功能已经标注清楚。

![1714207427903](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714207427903.png)

### 参考资料

[1]博客: *https://tc39.es/proposal-temporal/docs/*