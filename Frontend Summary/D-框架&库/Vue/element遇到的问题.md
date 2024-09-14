## Form

##### form表单输入框回车刷新页面

```js
# 原因：
	当一个form表单里只有一个输入框时,输入框内回车会默认触发表单的提交submit事件

# 解决：
	阻止表单的提交事件
    <el-form @submit.native.prevent="">    
    <el-form @submit.prevent=""> （element文档中 TIP提到的，不知道管不管用）
```

##### 时间组件date-picker 去掉“此刻”(now)二字

**第一种: 组件添加样式**

```js
<el-date-picker popper-class="no-atTheMoment" >
</el-date-picker>
// 在<el-date-picker>这个标签内加入popper-class="no-atTheMoment"，这个是官方提供的日期弹出层添加样式的方法，用这个classname做限制，不想显示“此刻”的，就加上这个，想显示的，就去掉它即可.
// el-date-picker 组件键 下拉列表插入到了body元素中,所以 <style scoped></style> 会不起作用因为在组件中找不到,所以把 scoped 去掉样式就会全局起作用了.
# element-ui
<style lang="scss">
    
.el-picker-panel.no-atTheMoment {
    .el-button--text.el-picker-panel__link-btn {
        display: none;
    }
}	    
</style>

# element-plus
<style lang="scss">
    
.no-atTheMoment {
  // 去掉 此刻
  .is-text.el-picker-panel__link-btn {
    display: none;
  }
}

.no-atTheMoment {
  // 去掉 footer
  .el-picker-panel__footer {
    display: none;
  }
}
</style>
```

**第二种: 全局引入样式**

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
  ],
  // 日期区间的规则
  dataRange: [
      {
          type: 'array',
          required: true,
          message:'请选择日期区间',
          fileds: {
              // type类型视情况而定，所以如果返回的是date就改成date，如果返回的是string就改成string
              0:{
                  type: 'date',
                  required: true,
                  message: '请选择开始日期'
              },
              1:{
                  type: 'date',
                  required: true,
                  message: '请选择结束日期'
              }
          }
      }
  ]
})
```

##### 阻止点击label触发表单控件的事件

```js
# 查询表单和新建表单有多个字段相同，所以做成一个公共的组件，出现问题的是查询和新建页面同时展示，点击新建的表单中的label，触发控件事件的却是查询页面，所以要 阻止点击label触发表单控件的事件

# element-plus
:deep(.el-form-item__label) {
    pointer-events:none;
}
```

##### DatePicker禁用日期

> disabled-date: 一个用来判断该日期是否被禁用的函数，接受一个Date对象作为参数，应该返回一个Boolean值
>
> disabled-hours：判断该小时是否被禁用的函数，但是和disabled-date略有不同，返回的是一个数组，数据里面包含的是被禁用的小时，如无禁用选项，返回空数组即可
>
> disabled-minutes：同上
>
> disabled-seconds：同上

###### 日期

```vue
# 第一种
<template>
...
	<el-date-picker 
     	type="date" 
  		format="yyyy 年 MM 月 dd 日" 
        value-format="yyyy-MM-dd"
        placeholder="选择日期"
        :picker-options="pickerOptions" // 或者 :disabled-date='disabledDate'
    >
    </el-date-picker>
...
</template>

<script>
export default {
    data(){
        return {
            pickerOptions:{
                disabledDate(time){
                    return time.getTime()<new Date().getTime()
                }
            }
        }
    }
}
</script>

# 第二种 (未测试)
<template>
...
	<el-date-picker 
     	type="date" 
  		format="yyyy 年 MM 月 dd 日" 
        value-format="yyyy-MM-dd"
        placeholder="选择日期"
        :disabled-date='disabledDate'
    >
    </el-date-picker>
...
</template>

<script>
export default {
    method: {
        disabledDate(time){
            return time.getTime()<new Date().getTime()
        }
    }
}
</script>
```

**注意：根据props传值判断是否禁用**

```vue
<template>
...
	<el-date-picker 
     	type="date" 
  		format="yyyy 年 MM 月 dd 日" 
        value-format="yyyy-MM-dd"
        placeholder="选择日期"
        :picker-options="pickerOptions"
    >
    </el-date-picker>
...
</template>

<script>
export default {
    props:['accountingDate'],
    data(){
        return {
            pickerOptions:{
                disabledDate(time){
                    return time.getTime()<new Date(this.accountingDate).getTime()
                    // 这样会报错，说 accountingDate not found
                    // 我们可以使用 watch监听accountingDate，给pickerOptions重新赋值
                }
            }
        }
    }
}
</script>

<script>
export default {
    props:['accountingDate'],
    data(){
        return {
            pickerOptions:{ }
        }
    },
    watch:{
        accountingDate:{
            handler(value){
                if(value) {
                   this.pickerOptions = { 
                       disabledDate(time){
                            return time.getTime()<new Date(this.accountingDate).getTime()
                        }
                    }
                }
            }
        }
    }
}
</script>
```

###### 时间(时分秒)

```vue
<template>
...
	<el-date-picker 
     	type="date" 
  		format="yyyy 年 MM 月 dd 日" 
        value-format="yyyy-MM-dd"
        placeholder="选择日期"
        :disabled-hours="disabledHours"
        :disabled-minutes="disabledMinutes"
        :disabled-seconds="disabledSeconds"
    >
    </el-date-picker>
...
</template>

<script>
export default {
    method:{
       disabledHours(){
           const arr=[]
           if(new Date(selectTimes.value).getTime()>Date.now()) return arr
           for(let i=0;i<24;i++){
               if(new Date().getHours() <= i) continue
               arr.push(i)
           }
           return arr
       }, 
       disabledMinutes(){
           const arr=[]
           if(new Date(selectTimes.value).getTime()>Date.now()) return arr
           for(let i=0;i<60;i++){
               if(new Date().getMinutes() <= i) continue
               arr.push(i)
           }
           return arr
       }, 
       disabledSeconds(){
           const arr=[]
           if(new Date(selectTimes.value).getTime()>Date.now()) return arr
           for(let i=0;i<60;i++){
               if(new Date().getSeconds() <= i) continue
               arr.push(i)
           }
           return arr
       }, 
    }
}
</script>
```

## Table

##### 修改表头样式

```js
# plus
<el-table 
	:header-cell-style="
		{
            background:'#fff',
            color:'black'
        }
	"
></el-table>
```

##### 一行展示超出提示

```js
# plus
<el-table :tooltip-options="{ placement:'top'}">
    <el-table-column 
		v-for="item in columns" 
		:key="item.label"
		:label="item.label"
		:prop="item.prop"
		:min-width="item.width"
		show-overflow-tooltip
	>
        <template #default="scope:any">
          <span>{{ scope.row[item.prop] }}</span>
        </template>
    </el-table-column>
	<el-table-column label="Operations" fixed="right">
         <template #default="scope:any">
          <el-button>xx</el-button>
        </template>
    </el-table-column>
</el-table>

```

##### 选中单元格

```js
# plus
<el-table :cell-class-name="tableCellClassName" @cell-click="handleCellClick">
    <el-table-column 
		v-for="item in columns" 
		:key="item.label"
		:label="item.label"
		:prop="item.prop"
		:min-width="item.width"
		show-overflow-tooltip
	>
        <template #default="scope:any">
          <template v-if="scope.row.index===tableClickRowIndex&&scope.column.index===tableClickColumnIndex">
          	<el-input />
          </template>
          <span v-else>{{ scope.row[item.prop] }}</span>
        </template>
    </el-table-column>
</el-table>

<script setup lang="ts">
    ...
	const tableClickRowIndex = ref()
	const tableClickColumnIndex = ref()
	const tableCellClassName= (value) => {// 这个函数有必要写吗？？
        const {row,column,rowIndex,columnIndex}=value
        row.index = rowIndex
        column.index = columnIndex
    }
    const handleCellClick = (row,column) => {
        tableClickRowIndex.value = row.index
        tableClickColumnIndex = column.index
    }
</script>
```

##### 弹窗中的表格有数据但不展示

```js
# 弹窗中的表格有时有数据，但是，就是不展示（当前浏览器版本71）
# 原因： 与弹窗的遮罩层有关，去掉弹窗原有的遮罩层，数据就可以正常显示
<el-dialog :model="false">
    <el-table></el-table>
</el-dialog>

<!-- 添加一个遮罩层 -->
<div class='xx' v-if='xxx' ></div>

.xx {
    position:fixed;
    top:0;
    right:0;
    bottom:0;
    left:0;
    background-color:rgba(0,0,0,.5);
    z-index:2000;
}
```

##### 设置单行溢出隐藏...

```js
<el-table-column show-overflow-tooltip></el-table-column>
```

##### 设置show-overflow-tooltip不生效

```js
# 不生效代码
<el-table-column show-overflow-tooltip>
    <div>xxx<div>
</el-table-column>

# 将div设置为行内元素 span 等或 template
```

## MessageBox 弹窗

##### 修改弹窗按钮样式

```js
# element-ui

this.$alert('content','title',{
    confirmButtonText: '确定',
    showClose: false,
    showCancelButton: false,
    closeOnClickModal: false,
    confirmButtonClass: 'confirmButtonClassName',
    callback: (action)=>{}
})

<style scoped>
	.confirmButtonClassName {
        background-color: #10ab7c !important;
    }
</style>
# 注意 样式写到 带有scoped 属性的style标签中，样式不生效。（原因可能是 scoped 限定了样式只能在当前组件生效，而 弹窗组件 编译后不属于该组件，所以不生效）
// 正确写法 在不携带属性scoped的style标签中 修改样式
<style>
	.confirmButtonClassName {
        background-color: #10ab7c !important;
    }
</style>

```

