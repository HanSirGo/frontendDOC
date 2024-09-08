项目中使用reactive时，赋值可以使用Object.assign

```js
condt obj = reactive({
	a:'',
    b:'',
    c:0,
    d:false
})

# before
const obj2 = {
    a:'ss',
    b:'vv',
    c:3,
    d:true
}
obj.a = obj2.a
obj.b = obj2.b
obj.c = obj2.c
obj.d = obj2.d

# 一个一个的赋值太繁琐
# 试一试 object.assign()

Object.assign(obj,obj2)
// 注意 key要一致
```