##### (1) **Qs**

```
传递参数要将参数序列化。
qs 是一个增加了一些安全性的查询字符串解析和序列化字符串的库。
```

 

```js
npm install qs
import qs from 'qs’

qs.parse()是将URL解析成对象的形式
qs.stringify()是将对象 序列化成URL的形式，以&进行拼接
```

另外qs还不止这点本事，这里附上一个详细的文档

https://blog.csdn.net/sansan_7957/article/details/82227040