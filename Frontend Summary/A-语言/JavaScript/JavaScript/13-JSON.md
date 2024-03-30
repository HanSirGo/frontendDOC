## JSON

> 在JavaScript中，JSON（JavaScript Object Notation）是一种轻量级的数据交换格式，其操作主要包括将JavaScript对象转换为JSON字符串以及将JSON字符串解析回JavaScript对象。

#### JSON的方法

```
1.JSON.parse()
2.JSON.stringify()
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