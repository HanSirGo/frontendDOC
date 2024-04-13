## Accounting.js

Accounting.js 是一个用于数字、货币和货币解析/格式化的小型 JavaScript 库。它是轻量级的，完全可本地化的，没有依赖关系，并且在客户端或服务器端都可以很好地工作。使用独立或作为 nodeJS/npm 和 AMD/requireJS 模块。

```js
// Default usage:
accounting.formatMoney(12345678); // $12,345,678.00

// European formatting (custom symbol and separators), can also use options object as second parameter:
accounting.formatMoney(4999.99, "€", 2, ".", ","); // €4.999,99

// Negative values can be formatted nicely:
accounting.formatMoney(-500000, "£ ", 0); // £ -500,000

// Simple `format` string allows control of symbol position (%v = value, %s = symbol):
accounting.formatMoney(5318008, { symbol: "GBP",  format: "%v %s" }); // 5,318,008.00 GBP
```

**Github：**https://github.com/openexchangerates/accounting.js

