## qs

qs是一个url参数转化（parse和stringify）的JavaScript库。可以把格式化的字符串转换为对象格式。

```js
var qs = require('qs');
var assert = require('assert');

var obj = qs.parse('a=c');
assert.deepEqual(obj, { a: 'c' });

var str = qs.stringify(obj);
assert.equal(str, 'a=c');
```

**Github：**https://github.com/ljharb/qs

