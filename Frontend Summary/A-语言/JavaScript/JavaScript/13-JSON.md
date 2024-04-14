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