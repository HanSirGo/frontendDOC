##### form表单输入框回车刷新页面

```js
# 原因：
	当一个form表单里只有一个输入款时,输入框内回车会默认触发表单的提交submit事件

# 解决：
	阻止表单的提交事件
    <el-form @submit.native.prevent="">
```

##### 时间组件date-picker 去掉“此刻”(now)二字

```js
# element-ui

// 官方文档没有提供去掉这个的属性，网上查了一下，主要是通过给这个标签添加css属性，display：none，来隐藏。但是我在组件内的<style>标签内，添加样式的修改不起作用，写导入符>>>也没效果，因此只能写在全局引入的css文件中。具体做法如下

1. 引用
<el-date-picker
                    v-model="postData.startDateTime"
                    type="datetime"
                    popper-class="no-atTheMoment"
                    value-format="yyyy-MM-dd HH:mm:ss"
                    :picker-options="pickerOptionsStartDateTime"
                    placeholder="开始时间"
                >
                </el-date-picker>
// 在<el-date-picker>这个标签内加入popper-class="no-atTheMoment"，这个是官方提供的日期弹出层添加样式的方法，用这个classname做限制，不想显示“此刻”的，就加上这个，想显示的，就去掉它即可。

2. 在全局(main.ts)引入的css文件中，添加

.el-picker-panel.no-atTheMoment {
    .el-button--text.el-picker-panel__link-btn {
        display: none;
    }
}
```

```js
# elment-plus
.no-atTheMoment {
  // 去掉 此刻
  .is-text.el-picker-panel__link-btn {
    display: none;
  }
}
```

##### 表单规则

```js
const rules = reactive({
  account: [
    // required是否必填,message不符合此规则时的提示信息,
    // trigger触发此条规则校验的时机，有两个值, blur 或 change,默认就是blur和change都会进行校验
    // min此字段的最小长度，max此字段的最大长度
    // pattern 正则表达式
    { required: true, message: '账户不能为空', trigger: 'blur' },
    { min: 6, max: 14, message: '用户名长度为6 - 14位' }
  ]
})

// 定义校验规则
const rules = reactive({
// 普通的校验规则
  account: [
    { required: true, message: '账户不能为空', trigger: 'blur' },
    { min: 6, max: 14, message: '用户名长度为6 - 14位' }
  ],

  password: [
  // 自定义校验规则
    {
      validator(rule, value, callback) {
        if (value[0] === '0') {
	      // 校验不通过
          return callback(new Error('密码字段的第一位不能是0'))
        } else {
          // 校验通过
          callback()
        }
      }
    }
  ]

})
```

