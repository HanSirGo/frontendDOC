### form表单输入框回车刷新页面

```js
# 原因：
	当一个form表单里只有一个输入款时,输入框内回车会默认触发表单的提交submit事件

# 解决：
	阻止表单的提交事件
    <el-form @submit.native.prevent="">
```

