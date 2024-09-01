## RegExp

### 正则表达式语法

> - #### 字符匹配
>
> - - • `.` 匹配任意单个字符，除了换行符；
>   - • `[...]` 匹配方括号中列举的任何一个字符；
>   - • `[^...]` 匹配除了方括号中列举的任何一个字符；
>   - • `\d` 匹配任何一个数字字符，相当于 `[0-9]`；
>   - • `\D` 匹配任何一个非数字字符，相当于 `[^0-9]`；
>   - • `\w` 匹配任何一个字母数字字符，相当于 `[a-zA-Z0-9_]`；
>   - • `\W` 匹配任何一个非字母数字字符，相当于 `[^a-zA-Z0-9_]`；
>   - • `\s` 匹配任何一个空白字符，包括空格、制表符、换行符等；
>   - • `\S` 匹配任何一个非空白字符；
>   - • `|` 表示或的关系，可以用来匹配多个模式中的一个。

> - #### 重复匹配
>
> - - • `*` 匹配前面的模式零次或多次；
>   - • `+` 匹配前面的模式一次或多次；
>   - • `?` 匹配前面的模式零次或一次；
>   - • `{n}` 匹配前面的模式恰好 n 次；
>   - • `{n,}` 匹配前面的模式至少 n 次；
>   - • `{n,m}` 匹配前面的模式至少 n 次但不超过 m 次。

> - #### 定位符
>
>   - `\b`：匹配一个单词边界，也就是指单词和空格间的位置；
>   - `\B`：匹配非单词边界；
>   - `^`：匹配开头，在多行匹配中匹配行开头；
>   - `$`：匹配结尾，在多行匹配中匹配行结尾；
>   - `(?=p)`：匹配 p 前面的位置；
>   - `(?!=p)`：匹配不是 p 前面的位置。

> - #### 分组
>
> - - • `()` 将其中的模式分组，可以在后面使用 `\1`、`\2` 等语法引用这些分组，表示匹配到的内容。例如：`/(\w+)\s+\1/` 匹配重复出现的单词。

> - #### 修饰符
>
>   - `g`：表示全局模式，即运用于所有字符串；
>   - `i`：表示不区分大小写，即匹配时忽略字符串的大小写；
>   - `m`：表示多行模式，强制 $ 和 ^ 分别匹配每个换行符。

除了上述语法之外，还有一些特殊字符和语法需要注意，比如转义符 `\`、非贪婪模式 `?` 等。同时，JavaScript 中正则表达式的方法也很多，例如 `test`、`exec`、`match`、`replace` 等，可以根据实际需求选择使用。

### 正则表达式常用方法

```js
# test() 方法：
用于测试一个字符串是否匹配某个模式。它返回一个布尔值，表示是否找到了匹配项。

const pattern = /hello/;
console.log(pattern.test("hello world")); // true
```

```js
# exec() 方法：
用于在一个指定字符串中执行一个搜索匹配。如果它找到了匹配项，则返回一个数组，否则返回 null。

const pattern = /world/;
const result = pattern.exec("hello world");
console.log(result[0]); // "world"
```

```js
# match() 方法：
用于在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。如果它找到了匹配项，则返回一个数组，否则返回 null。

const str = "The rain in Spain falls mainly in the plain";
const pattern = /ain/g;
const result = str.match(pattern);
console.log(result); // ["ain", "ain", "ain", "ain"]
```

```js
# search() 方法：
用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。如果找到了匹配项，则返回匹配的起始位置，否则返回 -1。

const pattern = /world/;
const position = "hello world".search(pattern);
console.log(position); // 6
```

```js
# replace() 方法：
用于在字符串中用一些字符替换另一些字符，或者替换一个与正则表达式匹配的子串。

const str = "Visit Microsoft!";
const pattern = /Microsoft/;
const newStr = str.replace(pattern, "W3Schools");
console.log(newStr); // "Visit W3Schools!"
```

### ES6 中新增的常用正则表达式方法

```js
# String.prototype.matchAll()：
返回一个迭代器，包含所有与正则表达式匹配的结果。每个结果是一个数组，包含匹配项及其捕获组。

示例：

const str = "Hello, World! Hello, Universe!";
const pattern = /Hello/g;
const matches = str.matchAll(pattern);

for (const match of matches) {
  console.log(match[0]); // 匹配项
}
```

```js
# RegExp.prototype.flags：
返回一个字符串，表示正则表达式的修饰符。

示例：

const pattern = /Hello/gi;
console.log(pattern.flags); // "gi"
```

```js
# RegExp.prototype.sticky：
表示正则表达式是否开启了粘性匹配模式。

示例：

const pattern = /Hello/y;
console.log(pattern.sticky); // true
```

```js
# RegExp.prototype.unicode
表示正则表达式是否开启了 Unicode 匹配模式。

示例：

const pattern = /Hello/u;
console.log(pattern.unicode); // true
```

```js
# String.prototype.startsWith()：
检查字符串是否以指定的字符串开始。

示例：

const str = "Hello, World!";
console.log(str.startsWith("Hello")); // true
```

```js
# String.prototype.endsWith()：
检查字符串是否以指定的字符串结尾。

示例：

const str = "Hello, World!";
console.log(str.endsWith("World!")); // true
```

```js
# String.prototype.includes()：
检查字符串是否包含指定的子字符串。

示例：

const str = "Hello, World!";
console.log(str.includes("World")); // true
```

### 常用正则表达式

```js
//（1）匹配 16 进制颜色值
var color = /#([0-9a-fA-F]{6}|[0-9a-fA-F]{3})/g;

//（2）匹配日期，如 yyyy-mm-dd 格式
var date = /^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[12][0-9]|3[01])$/;

//（3）匹配 qq 号
var qq = /^[1-9][0-9]{4,10}$/g;

//（4）手机号码正则
var phone = /^1[34578]\d{9}$/g;

//（5）用户名正则
var username = /^[a-zA-Z\$][a-zA-Z0-9_\$]{4,16}$/;

//（6）Email正则
var email = /^([A-Za-z0-9_\-\.])+\@([A-Za-z0-9_\-\.])+\.([A-Za-z]{2,4})$/;

//（7）身份证号（18位）正则
var cP = /^[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$/;

//（8）URL正则
var urlP= /^((https?|ftp|file):\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/;

// (9)ipv4地址正则
var ipP = /^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/;

// (10)//车牌号正则
var cPattern = /^[京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼使领A-Z]{1}[A-Z]{1}[A-Z0-9]{4}[A-Z0-9挂学警港澳]{1}$/;

// (11)强密码(必须包含大小写字母和数字的组合，不能使用特殊字符，长度在8-10之间)：var pwd = /^(?=.\d)(?=.[a-z])(?=.[A-Z]).{8,10}$/
```

