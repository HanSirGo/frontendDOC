## JSON

> 在JavaScript中，JSON（JavaScript Object Notation）是一种轻量级的数据交换格式，其操作主要包括将JavaScript对象转换为JSON字符串以及将JSON字符串解析回JavaScript对象。

#### JSON的方法

```
1.JSON.parse()
2.JSON.stringify()
```

##### JSON.parse()

```js
// JSON.parse() 的语法如下：

JSON.parse(text, reviver)
```

> - **text：** 必需， 一个有效的 JSON 字符串。
> - **reviver**：可选，一个转换结果的函数， 将为对象的每个成员调用此函数。

```js
const json = '{"name": "zhangsan", "age": 18, "city": "beijing"}';

const myJSON = JSON.parse(json);
 
console.log(myJSON.name, myJSON.age);  // zhangsan 18

// 我们可以启用 JSON.parse 的第二个参数 reviver，一个转换结果的函数，对象的每个成员都会调用此函数：

const json = '{"name": "zhangsan", "age": 18, "city": "beijing"}';

const myJSON = JSON.parse(json, (key, value) => {
  if(typeof value === "number") {
     return String(value).padStart(3, "0");
  }
  return value;
});
 
console.log(myJSON.name, myJSON.age);  // zhangsan 018
```

##### JSON.stringify()

```js
// JSON.stringify() 的语法如下：
JSON.stringify(value, replacer, space)
```

> - **value：** 必需， 要转换的 JavaScript 值（通常为对象或数组）。
> - **replacer：** 可选。用于转换结果的函数或数组。如果 replacer 为函数，则 JSON.stringify 将调用该函数，并传入每个成员的键和值。使用返回值而不是原始值。如果此函数返回 undefined，则排除成员。根对象的键是一个空字符串：""。如果 replacer 是一个数组，则仅转换该数组中具有键值的成员。成员的转换顺序与键在数组中的顺序一样。当 value 参数也为数组时，将忽略 replacer 数组。
> - **space：** 可选，文本添加缩进、空格和换行符，如果 space 是一个数字，则返回值文本在每个级别缩进指定数目的空格，如果 space 大于 10，则文本缩进 10 个空格。space 也可以使用非数字，如：`\t`。

```js
const json = {"name": "zhangsan", "age": 18, "city": "beijing"};

const myJSON = JSON.stringify(json);
 
console.log(myJSON);  // {"name":"zhangsan","age":18,"city":"beijing"}
```

######  格式化

```js
// 上面提到，可以使用JSON.stringify()来将 JSON 对象转换为字符串。它支持第二、三个参数。我们可以借助第二三个参数来格式化 JSON 字符串。正常情况下，格式化后的字符串长这样：

const json = {"name": "zhangsan", "age": 18, "city": "beijing"};

const myJSON = JSON.stringify(json);
 
console.log(myJSON);  // {"name":"zhangsan","age":18,"city":"beijing"}

// 添加第二三个参数：

const json = {"name": "zhangsan", "age": 18, "city": "beijing"};

const myJSON = JSON.stringify(json, null, 2);
 
console.log(myJSON);  

// 生成的字符串格式如下：

{
  "name": "zhangsan",
  "age": 18,
  "city": "beijing"
}

// 这里的 2 其实就是为返回值文本在每个级别缩进 2 个空格。

// 如果缩进是一个字符串而不是空格，就可以传入需要缩进的填充字符串：

const json = {"name": "zhangsan", "age": 18, "city": "beijing"};

const myJSON = JSON.stringify(json, null, "--");
 
console.log(myJSON);  

// 输出结果如下：

{
--"name": "zhangsan",
--"age": 18,
--"city": "beijing"
}
```

###### 隐藏属性

```js
// 我们知道JSON.stringify()支持第二个参数，用来处理 JSON 中的数据：
const user = {
  "name": "John",
  "password": "12345",
  "age": 30
};

console.log(JSON.stringify(user, (key, value) => {
    if (key === "password") {
       return;
    }
    return value;
}));

// 输出结果：{"name":"John","age":30}

// 可以将第二个参数抽离出一个函数：

function stripKeys(...keys) {
    return (key, value) => {
        if (keys.includes(key)) {
           return;
        }
        return value;
    };
}

const user = {
  "name": "John",
  "password": "12345",
  "age": 30,
  "gender": "male"
};

console.log(JSON.stringify(user, stripKeys('password', 'gender')))

// 输出结果：{"name":"John","age":30}
```

###### 过滤结果

```js
// 当 JSON 中的内容很多时，想要查看指定字段是比较困难的。可以借助JSON.stringify()的第二个属性来获取指定值，只需传入指定一个包含要查看的属性 key 的数组即可：

const user = {
    "name": "John",
    "password": "12345",
    "age": 30
}

console.log(JSON.stringify(user, ['name', 'age']))

// 输出结果：{"name":"John","age":30}
```

#### JSON的常用操作

##### 从JSON字符串转化为JavaScript对象

```js
var jsonString = '{"name": "John", "age": 30, "city": "New York"}';
var jsonObject = JSON.parse(jsonString);
console.log(jsonObject.name); // 输出: "John"
```

##### 将JavaScript对象转化为JSON字符串

```js
var obj = { name: "John", age: 30, city: "New York" };
var jsonString = JSON.stringify(obj);
console.log(jsonString); // 输出: "{"name":"John","age":30,"city":"New York"}"
```

##### 在JSON对象上进行操作

```js
# 添加/修改属性：
var jsonObj = JSON.parse(jsonString);
jsonObj.address = "123 Main St.";
var updatedJsonStr = JSON.stringify(jsonObj);

# 删除属性：
delete jsonObj.age;

# 访问属性值：
var value = jsonObj.city; // 获取 'city' 属性的值

# 遍历和过滤：
for (var key in jsonObj) {
if (jsonObj.hasOwnProperty(key)) {
    console.log(key + ": " + jsonObj[key]);
}
}

// 或者使用数组方法如map, filter等对JSON对象的值进行操作，前提是它是一个对象数组
var jsonArray = JSON.parse(jsonArrayStr);
var filteredItems = jsonArray.filter(item => item.type === 'important');
```

##### 排序JSON对象中的数组

```js
# 如果JSON对象包含一个数组，你可以先将其转化为JavaScript数组，然后对其进行排序：

var jsonArray = JSON.parse(jsonArrayStr).items;
jsonArray.sort((a, b) => a.value - b.value); // 对数值进行升序排序
```

##### 安全地解析JSON字符串

```js
# 考虑到JSON.parse()方法在解析无效JSON时会抛出错误，应捕获此异常以确保程序稳定运行：

try {
	var parsedData = JSON.parse(jsonString);
} catch (error) {
	console.error("Error parsing JSON string:", error);
}
```

#### 缺点

> - 只能处理纯JSON数据
>
> - 有几种情况会发生错误
>
> - 包含不能转成 JSON 格式的数据
>
> - 循环引用
>
> - undefined,NaN, -Infinity, Infinity 都会被转化成null
>
> - ***\*RegExp/函数\****不会拷贝
>
> - new Date()会被转成字符串

##### JSON.stringfy 会报错的情况

```js
# 1.循环引用：
当对象中存在循环引用时，JSON.stringify() 无法将其转换为 JSON 字符串，并抛出错误。例如：

let obj = {};
obj.prop = obj;
JSON.stringify(obj); // 报错：Converting circular structure to JSON
```

```js
# 2.不支持的数据类型：
JSON.stringify() 只能处理 JavaScript 支持的数据类型，如字符串、数字、布尔值、数组、对象和 null。

如果对象包含函数、RegExp、Date、undefined 或 Symbol 等不支持的数据类型，则会抛出错误。

例如：

let obj = {
  func: function() {}
};
JSON.stringify(obj); // 报错：TypeError: Converting circular structure to JSON
```

```js
# 3.大型对象的嵌套深度过大：
如果要序列化的对象存在很大的嵌套深度，超出了 JSON.stringify() 的默认堆栈大小限制，可能会导致堆栈溢出错误。例如：

let obj = {};
let currentObj = obj;
for (let i = 0; i < 100000; i++) {
  currentObj.prop = {};
  currentObj = currentObj.prop;
}
JSON.stringify(obj); // 报错：RangeError: Maximum call stack size exceeded
```

### JSON.stringify鲜为人知的技巧

![1723886043716](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723886043716.png)

在日常开发中，我们经常会使用到 JSON.stringify 这个方法，特别是在需要进行序列化（深拷贝）的时候。它可以把我们的对象转换成一个 JSON 字符串，这个方法确实非常方便，但它也有一些不常被注意到的缺点。

首先，JSON.stringify 的使用场景非常广泛，比如说当我们需要把对象保存到本地存储时，或者需要在前后端之间传输数据时，都会用到它。它就像是一个神奇的转换器，让我们可以轻松地处理复杂的数据结构。

然而，这个看似完美的方法也有它的局限性。比如说，它无法处理函数、undefined、Symbol 等特殊类型的数据，这些数据在转换成 JSON 字符串时会被忽略掉。此外，如果对象中存在循环引用，JSON.stringify 也会报错。

对于初学者来说，这些问题可能不太容易发现，因为大多数时候我们处理的数据都是简单的对象和数组。但在实际开发中，了解这些细节能够帮助我们更好地利用 JSON.stringify，同时避免一些潜在的坑。

#### **小心这些坑！**

在开发过程中，`JSON.stringify` 是我们常用的工具，但是它也有一些小坑，特别是处理某些特殊情况时。下面，我们通过几个例子来了解一下这些局限性。

#### **1. 函数问题**

如果对象的属性是函数，这个属性在序列化时会丢失。

```js
let person = {
    name: '小明',
    greet: function () {
        console.log(`你好，我是${ this.name }`)
    }
}

console.log(JSON.stringify(person)); // {"name":"小明"}
```

解释：在上面的例子中，`greet` 函数在转换成 JSON 字符串后被丢弃了。

#### **2. undefined 问题**

如果对象的属性值是 `undefined`，这个属性在转换后也会消失。

```js
let data = {
    age: undefined
}

console.log(JSON.stringify(data)); // {}
```

解释：在这个例子中，`age` 属性在转换后完全消失了。

#### **3. 正则表达式问题**

如果对象的属性是正则表达式，转换后会变成一个空对象。

```js
let settings = {
    name: '配置',
    pattern: /^a/,
    display: function () {
        console.log(`${ this.name }`)
    }
}

console.log(JSON.stringify(settings)); // {"name":"配置","pattern":{}}
```

解释：这里的 `pattern` 正则表达式在转换后变成了一个空对象 `{}`。

#### **4. 数组对象的问题**

上述问题在数组对象中同样会出现。

```js
let items = [
    {
        price: undefined
    }
]

console.log(JSON.stringify(items)); // [{}]
```

解释：在数组对象中，`price` 属性的 `undefined` 值在转换后也被丢弃了。

#### **JSON.stringify 的独特特性**

##### **1、特殊值的处理**

###### **对象属性中的特殊值**

当 `undefined`、函数和 `Symbol` 作为对象属性值时，`JSON.stringify` 会忽略它们。

```js
const data = {
  a: "文字",
  b: undefined,
  c: Symbol("符号"),
  fn: function() {
    return true;
  }
};

console.log(JSON.stringify(data)); // "{"a":"文字"}"
```

解释：在上面的例子中，`data` 对象的 `b`、`c` 和 `fn` 属性在序列化后都被忽略了，只剩下属性 `a` 被保留。

###### **数组元素中的特殊值**

当 `undefined`、函数和 `Symbol` 作为数组元素时，`JSON.stringify` 会将它们序列化为 `null`。

```js
const array = ["文字", undefined, function aa() { return true; }, Symbol('符号')];

console.log(JSON.stringify(array)); // "["文字",null,null,null]"
```

解释：在这个例子中，数组中的 `undefined`、函数和 `Symbol` 都被转换成了 `null`，而不是被忽略。

###### **独立的特殊值**

当 `undefined`、函数和 `Symbol` 被独立序列化时，`JSON.stringify` 会直接返回 `undefined`。

```js
console.log(JSON.stringify(function a (){console.log('a')})); // undefined
console.log(JSON.stringify(undefined)); // undefined
console.log(JSON.stringify(Symbol('符号'))); // undefined
```

解释：在这些情况下，`JSON.stringify` 并不会返回 JSON 字符串，而是直接返回 `undefined`，表示这些值无法被序列化。

##### **2、顺序**

在使用 `JSON.stringify` 进行序列化时，除了我们之前提到的特殊值处理外，还有一个需要注意的点就是对象属性的顺序问题。让我们通过具体的例子来了解这些特性。

###### **非数组对象的属性顺序**

对于非数组对象来说，属性的顺序在序列化后的 JSON 字符串中并不一定是按照我们定义的顺序出现的，尤其是当一些属性值被忽略时。

```js
const data = {
  a: "文字",
  b: undefined,
  c: Symbol("符号"),
  fn: function() {
    return true;
  },
  d: "更多文字"
};

console.log(JSON.stringify(data)); // "{"a":"文字","d":"更多文字"}"
```

解释：在这个例子中，`data` 对象中的 `b`、`c` 和 `fn` 属性由于特殊值的原因被忽略了，最终的 JSON 字符串中只剩下 `a` 和 `d` 属性，而且顺序并没有保证。

###### **数组元素的顺序**

对于数组来说，元素的顺序在序列化后是可以保证的，即使数组中包含 `undefined`、函数和 `Symbol` 这些特殊值，它们会被转换成 `null`，但顺序不会改变。

```js
const array = ["文字", undefined, function aa() { return true; }, Symbol('符号'), "更多文字"];

console.log(JSON.stringify(array)); // "["文字",null,null,null,"更多文字"]"
```

解释：在这个例子中，数组中的 `undefined`、函数和 `Symbol` 被转换成 `null`，但数组中元素的顺序保持不变。

##### **3、利用 toJSON 方法自定义序列化结果**

在使用 `JSON.stringify` 进行对象序列化时，有一个非常有趣且强大的特性——如果被转换的值中包含 `toJSON()` 方法，那么序列化的结果将由 `toJSON()` 方法返回的值决定，而忽略对象的其他属性。这为我们提供了很大的灵活性，可以自定义序列化结果。让我们通过具体的例子来了解这个特性。

###### **自定义序列化结果**

当一个对象包含 `toJSON()` 方法时，`JSON.stringify` 会调用这个方法，并使用其返回值作为最终的序列化结果。

```js
const data = {
  say: "你好，JSON.stringify",
  toJSON: function() {
    return "今天我学到了";
  }
};

console.log(JSON.stringify(data)); // "今天我学到了"
```

解释：在这个例子中，虽然 `data` 对象包含了 `say` 属性，但因为它定义了 `toJSON()` 方法，序列化时会调用这个方法，并使用它的返回值 `"今天我学到了"` 作为最终结果。

###### **实际应用场景**

这个特性可以在多种场景中应用，比如在对象序列化时需要隐藏某些敏感信息，或者只返回对象中的关键信息。

```js
const user = {
  name: "小明",
  password: "123456",
  toJSON: function() {
    return {
      name: this.name
    };
  }
};

console.log(JSON.stringify(user)); // "{"name":"小明"}"
```

解释：在这个例子中，`user` 对象通过 `toJSON()` 方法自定义了序列化结果，只返回 `name` 属性，隐藏了敏感的 `password` 信息。

##### **4、Date 对象的序列化技巧**

在使用 `JSON.stringify` 时，处理日期对象是一个常见的需求。幸运的是，`JSON.stringify` 可以很好地处理 `Date` 对象，因为 `Date` 对象本身实现了 `toJSON()` 方法。让我们通过具体例子来了解这一特性，并探讨如何在实际开发中灵活运用。

###### **日期对象的序列化**

当我们将 `Date` 对象传给 `JSON.stringify` 时，它会调用 `Date` 对象的 `toJSON()` 方法，该方法等同于 `Date.toISOString()`，返回一个标准的 ISO 字符串格式。

```js
const data = {
  now: new Date()
};

console.log(JSON.stringify(data)); // "{"now":"2024-06-16T12:43:13.577Z"}"
```

解释：在这个例子中，`data` 对象中的 `now` 属性是一个 `Date` 对象，序列化后变成了 ISO 8601 格式的字符串，表示日期和时间。

###### **Date 对象的 toJSON() 方法**

`Date` 对象的 `toJSON()` 方法实际上返回的是 `Date.toISOString()` 的结果，这使得日期在序列化后以字符串形式表示，并且格式统一，方便数据传输和存储。

```js
const now = new Date();

console.log(now.toJSON()); // "2024-06-16T12:43:13.577Z"
console.log(now.toISOString()); // "2024-06-16T12:43:13.577Z"
```

解释：无论是直接调用 `toJSON()` 还是 `toISOString()`，得到的结果都是同样的 ISO 字符串格式。

###### **实际应用场景**

在实际开发中，我们经常需要将日期对象转换为 JSON 字符串，以便在前后端之间传输数据或存储到数据库中。统一的 ISO 格式不仅简洁，还可以方便地在不同系统和语言之间解析和使用。

```js
const event = {
  name: "会议",
  date: new Date("2024-08-11T10:00:00Z")
};

console.log(JSON.stringify(event)); // "{"name":"会议","date":"2024-08-11T10:00:00.000Z"}"
```

解释：在这个例子中，我们创建了一个包含日期的事件对象，序列化后，日期属性被转换成 ISO 字符串，方便在网络上传输。

##### **5、的特殊数值处理：NaN、Infinity 和 null 的处理方式**

在使用 `JSON.stringify` 时，处理特殊数值也是一个需要注意的问题。`NaN`、`Infinity` 和 `null` 在序列化时都会被处理为 `null`。让我们通过一些具体的例子来详细了解这一特性。

###### **数值 NaN 和 Infinity 的序列化**

当我们将 `NaN` 或 `Infinity` 传给 `JSON.stringify` 时，它们会被转换成 `null`。

```
console.log(JSON.stringify(NaN)); // "null"
console.log(JSON.stringify(Infinity)); // "null"
```

解释：在这个例子中，无论是 `NaN` 还是 `Infinity`，都被序列化为 `"null"`，这是因为 JSON 不支持这两种特殊数值，所以将其转换为 `null` 来表示。

###### **null 的序列化**

当我们将 `null` 传给 `JSON.stringify` 时，它会被直接转换为字符串 `"null"`。

```js
console.log(JSON.stringify(null)); // "null"
```

解释：`null` 是 JSON 支持的一个特殊值，所以它在序列化时会被保留为 `"null"` 字符串。

###### **对象和数组中的特殊数值**

当 `NaN`、`Infinity` 和 `null` 作为对象属性值或数组元素时，它们会被转换为 `null`。

```js
const data = {
  num1: NaN,
  num2: Infinity,
  num3: null
};

console.log(JSON.stringify(data)); // "{"num1":null,"num2":null,"num3":null}"

const array = [NaN, Infinity, null];
console.log(JSON.stringify(array)); // "[null,null,null]"
```

解释：在这个例子中，`data` 对象中的 `num1` 和 `num2` 属性值，及数组中的 `NaN` 和 `Infinity` 元素，都被转换为 `null`。

###### **实际应用场景**

理解这些特性有助于我们在实际开发中正确处理数据，特别是在数据传输和存储时，避免因为特殊数值导致的数据不一致问题。

```js
const stats = {
  average: NaN,
  max: Infinity,
  min: null,
  values: [1, 2, NaN, 4, Infinity]
};

console.log(JSON.stringify(stats)); 
// "{"average":null,"max":null,"min":null,"values":[1,2,null,4,null]}"
```

解释：在这个统计数据对象中，所有的 `NaN` 和 `Infinity` 值在序列化后都被转换为 `null`，保证了 JSON 数据的一致性。

##### **6、包装对象处理：自动转换为原始值**

在使用 `JSON.stringify` 时，有一个很重要的特性是，布尔值、数字和字符串的包装对象在序列化时会自动转换为它们对应的原始值。这一特性有助于确保序列化后的 JSON 数据更简洁和易于使用。让我们通过具体的例子来深入了解这一特性。

###### **包装对象的序列化**

当我们将 `Number`、`String` 和 `Boolean` 包装对象传给 `JSON.stringify` 时，它们会被自动转换为对应的原始值。

```js
console.log(JSON.stringify([new Number(1), new String("false"), new Boolean(false)]));
// "[1,"false",false]"
```

解释：在这个例子中，`new Number(1)` 被转换为 `1`，`new String("false")` 被转换为 `"false"`，`new Boolean(false)` 被转换为 `false`。这些包装对象在序列化时都被简化为原始值，确保 JSON 数据的简洁性。

##### **7、枚举属性与非枚举属性**

在使用 `JSON.stringify` 进行对象序列化时，有一个关键的特性是：它只会序列化对象的可枚举属性（enumerable properties）。这意味着非枚举属性会被忽略，从而确保序列化结果的简洁和预期。让我们通过具体的例子来详细了解这一特性。

###### **枚举属性与非枚举属性的区别**

在 JavaScript 中，对象的属性可以被标记为枚举属性或非枚举属性。`JSON.stringify` 只会序列化枚举属性，而忽略非枚举属性。

```js
const data = Object.create(
    null,
    { 
        x: { value: 'json', enumerable: false }, 
        y: { value: 'stringify', enumerable: true } 
    }
);

console.log(JSON.stringify(data)); // "{"y":"stringify"}"
```

解释：在这个例子中，`data` 对象的 `x` 属性被标记为非枚举属性，`y` 属性被标记为枚举属性。`JSON.stringify` 在序列化时忽略了 `x` 属性，只保留了 `y` 属性。

###### **处理 Map、Set 等对象**

类似地，对于 `Map`、`Set` 等对象，`JSON.stringify` 也只会序列化它们的可枚举属性，而这些对象的特殊数据结构本身不会被直接序列化。

```js
const map = new Map();
map.set('a', 1);
map.set('b', 2);

const set = new Set();
set.add(1);
set.add(2);

console.log(JSON.stringify(map)); // "{}"
console.log(JSON.stringify(set)); // "{}"
```

解释：在这个例子中，`map` 和 `set` 对象的内部数据结构没有被序列化，而是返回了空对象。这是因为 `Map` 和 `Set` 的数据存储并不是作为对象的属性存在的。

###### **实际应用场景**

了解这个特性对于处理复杂对象结构非常重要，特别是在需要控制序列化结果的情况下。例如，可以通过定义非枚举属性来隐藏一些不需要序列化的内部数据。

```js
const user = {
    name: '小明',
    age: 25
};

Object.defineProperty(user, 'password', {
    value: '123456',
    enumerable: false
});

console.log(JSON.stringify(user)); // "{"name":"小明","age":25}"
```

解释：在这个例子中，我们通过 `Object.defineProperty` 将 `password` 属性标记为非枚举属性，从而在序列化时将其隐藏。

##### **8、JSON.parse(JSON.stringify()) 的局限性**

我们都知道，使用 `JSON.parse(JSON.stringify())` 是实现深克隆的最简单和直接的方法。然而，由于序列化的各种特性，这种方法在实际应用中会带来许多问题，尤其是当对象存在循环引用时，会导致错误。让我们通过一个具体的例子来详细了解这些局限性。

###### **循环引用的问题**

当对象存在循环引用时，`JSON.stringify()` 会抛出错误，因为 JSON 不支持循环结构。

```js
const obj = {
  name: "loopObj"
};
const loopObj = {
  obj
};
// 对象形成循环引用，创建了一个闭环
obj.loopObj = loopObj;

// 封装一个深克隆函数
function deepClone(obj) {
  return JSON.parse(JSON.stringify(obj));
}
// 执行深克隆，会抛出错误
deepClone(obj);
/**
VM44:9 Uncaught TypeError: Converting circular structure to JSON
    --> starting at object with constructor 'Object'
    |     property 'loopObj' -> object with constructor 'Object'
    --- property 'obj' closes the circle
    at JSON.stringify (<anonymous>)
    at deepClone (<anonymous>:9:26)
    at <anonymous>:11:13
 */
```

解释：在这个例子中，`obj` 和 `loopObj` 形成了一个循环引用，这导致 `JSON.stringify()` 在处理时抛出了 `TypeError` 错误。

###### **深克隆的替代方法**

为了安全地进行深克隆，特别是处理循环引用，我们需要使用更复杂的方法。以下是两种常见的替代方案：

###### **1. 使用递归和 Map 记录引用**

```js
function deepClone(obj, hash = new WeakMap()) {
  if (obj === null) return null;
  if (typeof obj !== "object") return obj;
  if (hash.has(obj)) return hash.get(obj);

  const cloneObj = Array.isArray(obj) ? [] : {};
  hash.set(obj, cloneObj);

  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      cloneObj[key] = deepClone(obj[key], hash);
    }
  }

  return cloneObj;
}

const newObj = deepClone(obj);
console.log(newObj);
```

解释：这个 `deepClone` 函数使用 `WeakMap` 来记录已经克隆过的对象，避免了循环引用的问题。

###### **2. 使用第三方库**

如果不想手动实现深克隆，可以使用现成的第三方库，如 `lodash` 提供的 `_.cloneDeep` 方法。

```js
const _ = require('lodash');

const newObj = _.cloneDeep(obj);
console.log(newObj);
```

解释：`lodash` 的 `_.cloneDeep` 方法能够处理大多数复杂情况，包括循环引用。

##### **9、Symbol 属性的序列化问题**

在使用 `JSON.stringify` 进行对象序列化时，有一个需要特别注意的点：如果对象的属性使用 `Symbol` 作为键，这些属性会被完全忽略，即使在 `replacer` 参数中显式包含也无效。让我们通过具体的例子来详细了解这一特性。

###### **Symbol 属性的序列化**

当对象的属性使用 `Symbol` 作为键时，`JSON.stringify` 会忽略这些属性，不会将它们包含在序列化结果中。

```js
const data = { [Symbol.for("json")]: "stringify" };

console.log(JSON.stringify(data)); // "{}"
```

解释：在这个例子中，`data` 对象的属性键是一个 `Symbol`，所以在序列化时，这个属性被完全忽略了。

###### **使用 replacer 参数**

即使我们在 `JSON.stringify` 的 `replacer` 参数中显式包含处理 `Symbol` 属性的逻辑，这些属性仍然会被忽略。

```js
const data = { [Symbol.for("json")]: "stringify" };

console.log(JSON.stringify(data, function(k, v) {
    if (typeof k === "symbol") {
      return v;
    }
  })); // "undefined"
```

解释：在这个例子中，`replacer` 函数试图检查 `Symbol` 类型的键并返回其值，但 `JSON.stringify` 仍然忽略了该属性，结果为undefined。

#### **第二个参数 replacer 的妙用**

在使用 `JSON.stringify` 进行对象序列化时，除了常见的用法，还有一些高级功能可以通过第二个参数 `replacer` 实现。`replacer` 可以是一个函数或数组，用于定制序列化的结果。让我们通过具体的例子来详细了解这些用法。

##### **1. 使用 replacer 参数作为函数**

当 `replacer` 参数是一个函数时，它会在每个属性值被序列化之前调用，类似于数组方法中的 `map` 和 `filter`。该函数接收两个参数：键和值。

```js
onst data = {
  a: "aaa",
  b: undefined,
  c: Symbol("dd"),
  fn: function() {
    return true;
  }
};

// 不使用 replacer 参数
console.log(JSON.stringify(data)); 
// "{"a":"aaa"}"

// 使用 replacer 参数作为函数
console.log(JSON.stringify(data, (key, value) => {
  switch (true) {
    case typeof value === "undefined":
      return "undefined";
    case typeof value === "symbol":
      return value.toString();
    case typeof value === "function":
      return value.toString();
    default:
      break;
  }
  return value;
}));
// "{"a":"aaa","b":"undefined","c":"Symbol(dd)","fn":"function() {\n    return true;\n  }"}"
```

解释：在这个例子中，`replacer` 函数将 `undefined`、`Symbol` 和函数转换为字符串，使得它们能够被序列化。

##### **2. 第一次调用 replacer 函数的特殊情况**

当 `replacer` 函数被第一次调用时，传入的第一个参数并不是对象的第一个键值对，而是一个空字符串作为键，整个对象作为值。

```js
const data = {
  a: 2,
  b: 3,
  c: 4,
  d: 5
};

console.log(JSON.stringify(data, (key, value) => {
  console.log(value);
  return value;
}));
// The first argument passed to the replacer function is 
// {"":{a: 2, b: 3, c: 4, d: 5}}
// 2
// 3
// 4
// 5
// {a: 2, b: 3, c: 4, d: 5}   
```

##### **3. 使用 replacer 函数实现对象的 map 功能**

我们可以利用 `replacer` 函数手动实现类似于数组 `map` 方法的功能，遍历对象的每个属性并对其进行操作。

```js
const data = {
  a: 2,
  b: 3,
  c: 4,
  d: 5
};

const objMap = (obj, fn) => {
  if (typeof fn !== "function") {
    throw new TypeError(`${fn} is not a function !`);
  }
  return JSON.parse(JSON.stringify(obj, fn));
};

console.log(objMap(data, (key, value) => {
  if (value % 2 === 0) {
    return value / 2;
  }
  return value;
}));
// {a: 1, b: 3, c: 2, d: 5}
```

##### **4. 使用 replacer 参数作为数组**

当 `replacer` 参数是一个数组时，数组中的值表示要被序列化到 JSON 字符串中的属性名。

```js
const jsonObj = {
  name: "JSON.stringify",
  params: "obj,replacer,space"
};

// 仅保留 params 属性的值
console.log(JSON.stringify(jsonObj, ["params"]));
// "{"params":"obj,replacer,space"}"
```

解释：在这个例子中，`replacer` 数组指定只序列化 `params` 属性，其他属性会被忽略。

#### **第三个参数：控制输出格式的 space 参数**

在使用 `JSON.stringify` 进行对象序列化时，第三个参数 `space` 用于控制生成的 JSON 字符串中的空格和缩进。这个参数可以显著提高输出结果的可读性。让我们通过具体的例子来了解 `space` 参数的作用和用法。

##### **1. space 参数的基本用法**

`space` 参数可以是一个数字或字符串。数字表示每一级嵌套的缩进空格数，字符串则用于每一级嵌套的缩进字符。

```js
const tiedan = {
  name: "Jhon",
  describe: "JSON.stringify()",
  emotion: "like"
};

// 使用 "--" 作为缩进字符
console.log(JSON.stringify(tiedan, null, "--"));
/* 输出结果如下:
{
--"name": "Jhon",
--"describe": "JSON.stringify()",
--"emotion": "like"
}
*/

// 使用 2 个空格作为缩进字符
console.log(JSON.stringify(tiedan, null, 2));
/* 输出结果如下:
{
  "name": "Jhon",
  "describe": "JSON.stringify()",
  "emotion": "like"
}
*/
```

解释：在第一个例子中，`space` 参数是字符串 `"--"`，所以每一级嵌套使用两个连字符作为缩进。在第二个例子中，`space` 参数是数字 `2`，所以每一级嵌套使用两个空格作为缩进。

##### **2. 数字作为 space 参数**

当 `space` 参数是数字时，它表示每一级嵌套的缩进空格数，最大值为 10。

```js
const data = {
  level1: {
    level2: {
      level3: "deep"
    }
  }
};

// 使用 4 个空格作为缩进字符
console.log(JSON.stringify(data, null, 4));
/* 输出结果如下:
{
    "level1": {
        "level2": {
            "level3": "deep"
        }
    }
}
*/
```

解释：在这个例子中，`space` 参数是数字 `4`，所以每一级嵌套使用四个空格作为缩进。

### JSON.stringify规则

**1. undefined、Function 和 Symbol 不是有效的 JSON 值。**

如果在转换过程中遇到任何此类值，它们要么被省略（当在对象中找到时），要么更改为 null（当在数组中找到时）。

当传入像 JSON.stringify(function() {}) 或 JSON.stringify(undefined) 这样的“纯”值时，JSON.stringify() 可以返回 undefined。

![1713086152777](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713086152777.png)

**2.Boolean、Number、String对象在字符串化过程中被转换为对应的原始值，符合传统的转换语义。**

![1713086168259](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713086168259.png)

**3.所有以符号为键的属性将被完全忽略，即使在使用替换函数时也是如此。**

![1713086183846](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713086183846.png)

**4. 数字 Infinity 和 NaN，以及值 null，都被认为是 null。**

![1713086198466](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713086198466.png)

**5. 如果该值有一个 toJSON() 方法，它负责定义哪些数据将被序列化。**

![1713086214850](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713086214850.png)

**6. Date实例通过返回字符串实现toJSON()函数（同date.toISOString()）。** 

因此，它们被视为字符串。

![1713086230745](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713086230745.png)

**7. 在包含循环引用的对象上执行此方法会抛出错误。**

![1713086246253](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713086246253.png)

**8. 所有其他对象实例（包括 Map、Set、WeakMap 和 WeakSet）将只序列化它们的可枚举属性。**

![1713086263482](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713086263482.png)

**9.尝试转换BigInt类型的值时抛出错误。**

![1713086276565](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713086276565.png)

**自己实现 JSON.stringify**

理解功能的最好方法是自己去实现它。 下面我写了一个简单的函数来模拟JSON.stringify。

```js
const jsonstringify = (data) => {
  // Check if an object has a circular reference
  const isCyclic = (obj) => {
  // Use the Set data type to store detected objects
  let stackSet = new Set()
  let detected = false

  const detect = (obj) => {
    // If it is not an object type, you can skip it directly
    if (obj && typeof obj != 'object') {
      return
    }
    // When the object to be checked already exists in the stackSet, it means that there is a circular reference
    if (stackSet.has(obj)) {
      return detected = true
    }
    // Save the current obj as a stackSet
    stackSet.add(obj)

    for (let key in obj) {
      if (obj.hasOwnProperty(key)) {
        detect(obj[key])
      }
    }
    // After the level detection is completed, delete the current object to prevent misjudgment
    /*
      For example:
      an object's attribute points to the same reference. 
      If it is not deleted, it will be regarded as a circular reference
      let tempObj = {
        name: 'fatfish'
      }
      let obj4 = {
        obj1: tempObj,
        obj2: tempObj
      }
    */
    stackSet.delete(obj)
  }

  detect(obj)

  return detected
}

  // 7#:
  // Executing this method on an object that contains a circular reference throws an error.

  if (isCyclic(data)) {
    throw new TypeError('Converting circular structure to JSON')
  }

  // 9#: An error is thrown when trying to convert a value of type BigInt
  // An error is thrown when trying to convert a value of type bigint
  if (typeof data === 'bigint') {
    throw new TypeError('Do not know how to serialize a BigInt')
  }

  const type = typeof data
  const commonKeys1 = ['undefined', 'function', 'symbol']
  const getType = (s) => {
    return Object.prototype.toString.call(s).replace(/\[object (.*?)\]/, '$1').toLowerCase()
  }

  // not an object
  if (type !== 'object' || data === null) {
    let result = data
    // 4#：The numbers Infinity and NaN, as well as the value null, are all considered null.
    if ([NaN, Infinity, null].includes(data)) {
      result = 'null'
      // 1#：undefined, Function, and Symbol are not valid JSON values. 
      // If any such values are encountered during conversion they are either omitted (when found in an object) or changed to null (when found in an array). 
      // JSON.stringify() can return undefined when passing in "pure" values like JSON.stringify(function() {}) or JSON.stringify(undefined).
    } else if (commonKeys1.includes(type)) {
      return undefined
    } else if (type === 'string') {
      result = '"' + data + '"'
    }

    return String(result)
  } else if (type === 'object') {
    // 5#: If the value has a toJSON() method, it's responsible to define what data will be serialized.
    // 6#: The instances of Date implement the toJSON() function by returning a string (the same as date.toISOString()). 
    // Thus, they are treated as strings.
    if (typeof data.toJSON === 'function') {
      return jsonstringify(data.toJSON())
    } else if (Array.isArray(data)) {
      let result = data.map((it) => {
        // 1#: If any such values are encountered during conversion they are either omitted (when found in an object) or changed to null (when found in an array). 
        return commonKeys1.includes(typeof it) ? 'null' : jsonstringify(it)
      })

      return `[${result}]`.replace(/'/g, '"')
    } else {
      // 2#：Boolean, Number, and String objects are converted to the corresponding primitive values during stringification, in accord with the traditional conversion semantics.
      if (['boolean', 'number'].includes(getType(data))) {
        return String(data)
      } else if (getType(data) === 'string') {
        return '"' + data + '"'
      } else {
        let result = []
        // 8#: All the other Object instances (including Map, Set, WeakMap, and WeakSet) will have only their enumerable properties serialized.
        Object.keys(data).forEach((key) => {
          // 3#: All Symbol-keyed properties will be completely ignored, even when using the replacer function.
          if (typeof key !== 'symbol') {
            const value = data[key]
            // 1#: undefined, Function, and Symbol are not valid JSON values.
            if (!commonKeys1.includes(typeof value)) {
              result.push(`"${key}":${jsonstringify(value)}`)
            }
          }
        })

        return `{${result}}`.replace(/'/, '"')
      }
    }
  }
}
```

还有一个测试

```js
// 1. Test basic features
console.log(jsonstringify(undefined)) // undefined 
console.log(jsonstringify(() => { })) // undefined
console.log(jsonstringify(Symbol('fatfish'))) // undefined
console.log(jsonstringify((NaN))) // null
console.log(jsonstringify((Infinity))) // null
console.log(jsonstringify((null))) // null
console.log(jsonstringify({
  name: 'fatfish',
  toJSON() {
    return {
      name: 'fatfish2',
      sex: 'boy'
    }
  }
}))
// {"name":"fatfish2","sex":"boy"}

// 2. Compare with native JSON.stringify
console.log(jsonstringify(null) === JSON.stringify(null));
// true
console.log(jsonstringify(undefined) === JSON.stringify(undefined));
// true
console.log(jsonstringify(false) === JSON.stringify(false));
// true
console.log(jsonstringify(NaN) === JSON.stringify(NaN));
// true
console.log(jsonstringify(Infinity) === JSON.stringify(Infinity));
// true
let str = "fatfish";
console.log(jsonstringify(str) === JSON.stringify(str));
// true
let reg = new RegExp("\w");
console.log(jsonstringify(reg) === JSON.stringify(reg));
// true
let date = new Date();
console.log(jsonstringify(date) === JSON.stringify(date));
// true
let sym = Symbol('fatfish');
console.log(jsonstringify(sym) === JSON.stringify(sym));
// true
let array = [1, 2, 3];
console.log(jsonstringify(array) === JSON.stringify(array));
// true
let obj = {
  name: 'fatfish',
  age: 18,
  attr: ['coding', 123],
  date: new Date(),
  uni: Symbol(2),
  sayHi: function () {
    console.log("hello world")
  },
  info: {
    age: 16,
    intro: {
      money: undefined,
      job: null
    }
  },
  pakingObj: {
    boolean: new Boolean(false),
    string: new String('fatfish'),
    number: new Number(1),
  }
}
console.log(jsonstringify(obj) === JSON.stringify(obj)) 
// true
console.log((jsonstringify(obj)))
// {"name":"fatfish","age":18,"attr":["coding",123],"date":"2021-10-06T14:59:58.306Z","info":{"age":16,"intro":{"job":null}},"pakingObj":{"boolean":false,"string":"fatfish","number":1}}
console.log(JSON.stringify(obj))
// {"name":"fatfish","age":18,"attr":["coding",123],"date":"2021-10-06T14:59:58.306Z","info":{"age":16,"intro":{"job":null}},"pakingObj":{"boolean":false,"string":"fatfish","number":1}}

// 3. Test traversable objects
let enumerableObj = {}

Object.defineProperties(enumerableObj, {
  name: {
    value: 'fatfish',
    enumerable: true
  },
  sex: {
    value: 'boy',
    enumerable: false
  },
})

console.log(jsonstringify(enumerableObj))
// {"name":"fatfish"}

// 4. Testing circular references and Bigint

let obj1 = { a: 'aa' }
let obj2 = { name: 'fatfish', a: obj1, b: obj1 }
obj2.obj = obj2

console.log(jsonstringify(obj2))
// TypeError: Converting circular structure to JSON
console.log(jsonStringify(BigInt(1)))
// TypeError: Do not know how to serialize a BigInt
```

### JSON.stringify的高级用法

我们常用JSON.stringify()方法将一个对象/值转化为JSON字符串，比如：在本地存储一个对象时，由于localStorage/sessionStorage只能存储字符串，所以我们可以使用JSON.stringify()方法将对象转化字符串进行存储，使用时再通过JSON.parse()转换回对象即可。

那么，你使用过JSON.stringify()方法的第二、第三个参数吗？他们的功能同样强大，此外，在使用过程中又有哪些坑需要注意呢？

本文将深度解析JSON.stringify()的功能、注意事项与使用场景

**基础用法**

先看一个简单的示例，下文将使用这个对象：

```

const obj = {
  key1: 'value-1',
  arr1: [
    'item1',
    'item2'
  ],
  subObj: {
    key2: 2,
    arr2: [
      'item3'
    ]
  }
}

console.log('JSON: ', JSON.stringify(obj))
```

输出：

![1723888675827](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723888675827.png)

**第二个参数：过滤、转换**

第二个参数接收内容可以是：数组、函数、null、不传入。接收一个数组或函数时，可以控制哪些内容可以被序列化，和被如何序列化；不传入或为null时，会序列化对象的所有属性（如上文）；除此之外，传入其他类型值均会被忽略（会序列化对象的所有属性）。

**当接收一个数组时**，只序列化该数组中包含的属性名：

```
console.log('JSON: ', JSON.stringify(obj, ['arr1', 'subObj']))
```

输出：

![1723888710912](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723888710912.png)

若只想序列化对象的指定属性，可以使用数组参数，但需要注意的是，数组**仅适用于顶级对象的属性**，而无法实现子对象的序列化，如上述subObj子对象，无法正确序列化。

**当接收一个函数时**，不仅可以更加灵活的控制需要被序列化的内容（如上述数组参数实现不了的子对象序列化），还可以自定义源对象被如何序列化。

先通过两个例子看一下函数接收的参数是什么：

**例一**

```

const replacer = (key, value) => {
  console.log('key: ', key, '\t', 'value: ', value)
  return value
}
console.log('JSON: ', JSON.stringify(obj, replacer))
```

输出：

![1723888747387](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723888747387.png)

**例二**

```

let count = 0
const replacer = (key, value) => {
  console.log('key: ', key, '\t', 'value: ', value)
  count++
  if(count > 5) {  // 控制结束条件，否则会无限循环
    return value
  }
  return { [`customKey-${count}`]: `customValue-${count}` }
}
console.log('JSON: ', JSON.stringify(obj, replacer))
```

输出：

![1723888770777](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723888770777.png)

可见，第一次收到的key值是一个空字符串，第一次收到的value就是源对象，随后，返回值中每个对象的属性与值会被依次传入，直到全部遍历完成（深度优先），并根据return值生成JSON.stringify()序列化的结果。

从上述两个例子已知：当return接收到的value时，对应的属性会被加入序列化结果中；当return一个对象（value也可能是一个对象）时，会将接收到的key值加入序列化结果的属性中，对应的属性值是继续对return的对象执行replacer函数的结果。

特殊的，若return的对象是一个函数，会被忽略，不会被加入序列化结果：

```

const replacer = (key, value) => {
  if(key === 'key1') {
    return () => {
      console.log('a function')
    }
  }
  return value
}
console.log('JSON: ', JSON.stringify(obj, replacer))
```

输出：

![1723888798149](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723888798149.png)

下面我们再通过一个例子看看其他各种return值都会产生什么样的效果：

```

const replacer = (key, value) => {
  switch(key) {
    case 'key1':
      return 1
    case '0':
      return true
    case '1':
      return 'custom-string'
    case 'key2':
      return undefined
    case 'arr2':
      return null
    default:
      return value
  }
}
console.log('JSON: ', JSON.stringify(obj, replacer))
```

输出：

![1723888825548](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723888825548.png)

根据结果我们总结其他情况的return值如下：

- 返回Number类型：会将返回值代替原value值；

- 返回Boolean类型：会将返回值代替原value值；

- 返回String类型：会将返回值代替原value值；

- 返回undefined：会忽略当前属性，不加入序列化结果（**特殊情况：对数组中的元素返回undefined，见下一个例子**）；

- 返回null：会将null代替原value值。

  

**特殊情况：对数组中的元素返回undefined：**

```
const replacer = (key, value) => {
  switch(key) {
    case '0':
      return undefined
    case '1':
    case '':
    case 'arr1':
      return value
    default:
      return undefined
  }
}
console.log('JSON: ', JSON.stringify(obj, replacer))
```

输出：

![1723888854867](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723888854867.png)

对数组arr1索引为0的元素返回undefined，序列化的结果中并没有删除此元素，而是转化为了null，所以，我们**不能使用return undefined的方式从数组中移除某个元素**。

如果就是需要移除数组的元素怎么实现呢？可以使用前文讲述的自定义return对象的方法：

```
const replacer = (key, value) => {
  switch(key) {
    case '0':
    case '1':
    case '':
      return value
    case 'arr1':
      value.splice(0, 1)
      return value
    default:
      return undefined
  }
}
console.log('JSON: ', JSON.stringify(obj, replacer))
```

输出：

![1723889186281](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723889186281.png)

**第三个参数：缩进**

这个参数是美化的作用，通过添加缩进（**至多缩进10个字符**）增强序列化结果的可读性，它的有效值类型是Number（正整数）或String。

**当值为Number类型时**，表示序列化结果缩进的字符数：

```
console.log('JSON: ', JSON.stringify(obj, null, 2))
```

输出：

![1723889216336](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723889216336.png)

```
console.log('JSON: ', JSON.stringify(obj, null, 12))
```

输出：

![1723889234235](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723889234235.png)

**当值为String类型时**，值的长度为序列化结果缩进的字符数，并会使用该值补全缩进：

```
console.log('JSON: ', JSON.stringify(obj, null, ' '))
```

输出：

![1723889253813](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723889253813.png)



```
console.log('JSON: ', JSON.stringify(obj, null, '>>'))
```

输出：

![1723889272985](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723889272985.png)

```
console.log('JSON: ', JSON.stringify(obj, null, '0123456789AB'))
```

输出：

![1723892214273](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723892214273.png)

另外，'\t'、'\n'也会实现相应的格式效果，大家可以尝试一下哦~

**toJSON()**

若源对象有自己的toJSON()方法，那么会使用toJSON()的返回值进行序列化（返回值会覆盖源对象），replacer作用于toJSON()的返回值：

```

const obj = {
  key1: 'value-1',
  arr1: [
    'item1',
    'item2'
  ],
  subObj: {
    key2: 2,
    arr2: [
      'item3'
    ]
  },
  toJSON() {
    return {
      newKey1: 'newValue-1',
      newSubObj: this.subObj
    }
  }
}

const replacer = (key, value) => {
  switch(key) {
    case 'newKey1':
      return 'A'
    case 'key2':
      return 'B'
    default:
      return value
  }
}
console.log('JSON: ', JSON.stringify(obj, replacer, 2))
```

输出：

![1723892249046](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723892249046.png)

若子对象有toJSON()方法，其返回值也同样会覆盖子对象原来的值：

```

const obj = {
  key1: 'value-1',
  arr1: [
    'item1',
    'item2'
  ],
  subObj: {
    key2: 2,
    arr2: [
      'item3'
    ],
    toJSON() {
      return 'new subObj value'
    }
  }
}

const replacer = (key, value) => {
  switch(key) {
    case 'newKey1':
      return 'A'
    case 'key2':
      return 'B'
    default:
      return value
  }
}
console.log('JSON: ', JSON.stringify(obj, replacer, 2))
```

输出：

![1723892291756](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723892291756.png)

**注意：无法序列化的情况**

**1、循环引用**

若源对象中包含循环引用，则无法使用JSON.stringify()进行序列化，会抛出错误：

```

const subObj = {}
const obj = {
  key1: 'value-1',
  arr1: [
    'item1',
    'item2'
  ],
  subObj
}
subObj.value = obj

console.log('obj: ', obj)
console.log('JSON: ', JSON.stringify(obj, null, 2))
```

输出：

![1723892326379](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723892326379.png)

**2、不可枚举的属性值**

不可枚举的属性在JSON.stringify()序列化时将被忽略：

```
const obj = Object.create(null, {
  key1: {
    value: 'value-1',
    enumerable: true
  },
  arr1: {
    value: [
      'item1',
      'item2'
    ],
    enumerable: false
  },
  subObj: {
    value: {
      key2: 2,
      arr2: [
        'item3'
      ]
    },
    enumerable: true
  }
})

console.log('obj: ', obj)
console.log('JSON: ', JSON.stringify(obj, null, 2))
```

输出：

![1723892358291](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723892358291.png)

**3、NaN、Infinity**

源对象中若包含NaN或Infinity值，在序列化时会转为null：

```

const obj = {
  key1: NaN,
  arr1: [
    NaN,
    'item2'
  ],
  subObj: {
    key2: Infinity,
    arr2: [
      Infinity
    ]
  }
}

console.log('JSON: ', JSON.stringify(obj, null, 2))
```

输出：

![1723892393957](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723892393957.png)

**4、undefined、symbol、函数**

若在非数组中包含undefined或symbol或函数，则该属性在JSON.stringify()序列化时将被忽略，若在数组中包含undefined或symbol或函数，则该元素在JSON.stringify()序列化时将被转为null：

```

const obj = {
  key1: () => console.log('function A'),
  arr1: [
    () => console.log('function B'),
    undefined
  ],
  subObj: {
    key2: undefined,
    key3: Symbol('A'),
    arr2: [
      Symbol('B')
    ]
  }
}

console.log('JSON: ', JSON.stringify(obj, null, 2))
```

输出：

![1723893244011](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723893244011.png)

**场景示例一：对象的深拷贝**

使用JSON.stringify()序列化对象，然后再使用JSON.parse()反序列化，即可根据序列化的结果创建一个相同的新对象。

**但需要注意**：若源对象中存在上文中的几种无法序列化的值，在JSON.stringify()序列化是被改变，相应的，在使用JSON.parse()后也就无法在得到原值，此时不适用通过JSON.parse(JSON.stringify())的方式来进行深拷贝。

**适用**

```

const obj = {
  key1: 'value-1',
  arr1: [
    'item1',
    'item2'
  ],
  subObj: {
    key2: 2,
    arr2: [
      'item3'
    ]
  }
}

console.log('深拷贝: ', JSON.parse(JSON.stringify(obj)))
```

输出：

![1723893277283](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723893277283.png)

**不适用**

```

const obj = {
  key1: undefined,
  arr1: [
    () => console.log('function A'),
    'item2'
  ],
  subObj: {
    key2: 2,
    arr2: [
      'item3'
    ]
  }
}

console.log('深拷贝: ', JSON.parse(JSON.stringify(obj)))
```

输出：

![1723893315066](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723893315066.png)

**场景示例二：数组的全等判断**

我们知道，即使是两个元素值与顺序完全相同的数组，在使用全等判断时结果也是false，因为即使值相同，实际上也是两个不同的对象。

这时，若使用JSON.stringify()序列化后再执行判断，就可以判断两个数组的元素与顺序是否完全一致，非常方便：

```

const arrA = [1, 2, 3]
const arrB = [1, 2, 3]

console.log('原数组: ', arrA === arrB)
console.log('序列化后: ', JSON.stringify(arrA) === JSON.stringify(arrB))
```

输出：

![1723893342673](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723893342673.png)

对象的全等判断也是同理（注意属性的顺序需要一致）。

最后，使用这些方法时都需要时刻注意几种无法序列化的情况哦!