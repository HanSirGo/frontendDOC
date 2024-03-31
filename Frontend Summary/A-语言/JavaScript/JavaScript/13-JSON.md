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



#### 