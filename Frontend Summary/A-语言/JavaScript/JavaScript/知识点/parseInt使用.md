# parseInt

> 根据 MDN 文档，“parseInt(string, radix) 函数解析字符串参数并返回指定基数（数学数字系统中的基数）的整数。”

语法

```js
parseInt(string)
parseInt(string, radix)
```

例如：

```js
parseInt('0.5') // 0
parseInt('0.5') // 0
parseInt('0.05') // 0
parseInt('0.005') // 0
parseInt('0.0005') // 0
parseInt('0.00005') // 0
parseInt('0.000005') // 0
parseInt('015') // 15
parseInt('015', 8) // 13
parseInt('15px', 10) // 15
```

**parseInt如何转换数字**

```js
// parseInt(0.0000005) === 5

# 第一步？将数字转换为字符串。

// 让我们使用 String 函数检查基于字符串的值，看看每个值的输出是什么：

String(0.5);      // => '0.5'
String(0.05);     // => '0.05'
String(0.005);    // => '0.005'
String(0.0005);   // => '0.0005' 
String(0.00005);  // => '0.00005'
String(0.000005); // => '0.000005'
String(0.0000005); // => '5e-7' pay attention here

# 第二步是进行舍入操作

// parseInt 只能将字符串的前导部分解释为整数值；它忽略任何不能解释为整数表示法一部分的代码单元，并且没有给出任何此类代码单元被忽略的指示。

parseInt(0.0000005)
parseInt('5e-7') // 5

// 最后，答案将仅返回 5，因为它是直到非字符 e 为止唯一一个数字字符，因此其余的 e-7 将被丢弃。”
```

**如何安全地获取浮点数的整数部分？**

建议使用以下 Math.floor() 函数：

```js
Math.floor(0.5);      // => 0
Math.floor(0.05);     // => 0
Math.floor(0.005);    // => 0
Math.floor(0.0005);   // => 0
Math.floor(0.00005);  // => 0
Math.floor(0.000005); // => 0
Math.floor(0.0000005); // => 0
```

解释一下为什么 parseInt(99999999999999999999999999) 等于 1 吗？

