## vue覆盖elementui样式的几种方式

#### 1、去掉 scoped 提升样式至全局。

但是这样的话需要增加命名空间以解决污染问题。

#### 2、使用深度选择器。

当你子组件使用了 scoped 但在父组件又想修改子组件的样式可以 通过 >>> 来实现：

```xml
<style scoped>
>>>.el-checkbox__input > .el-checkbox__inner {
    display:none;
}
</style>
```

#### 3、使用/deep/ 或者 ::v-deep 实现

当使用了sass等css预处理语言时， >>> 可能不会生效，此时可以使用 /deep/ 或者 ::v-deep替换，它的作用跟 >>> 时一样的。

```xml
<style lang="scss" scoped>
/deep/ .el-checkbox__input > .el-checkbox__inner {
    display:none;
}
</style>

<style lang="scss" scoped>
.a{
 ::v-deep .b { 
  /* ... */ 
 }
} 
</style>
```

#### 4、 部分样式无效，比如弹框或者模态框

有些样式覆盖无效，在scoped的style中写是无效的，因为ElementUI组件不可以给样式添加scoped，因此必须去掉scoped；但是去掉scoped后不满足单组件的CSS。
 解决方案
 1、附加在没有scoped的style中
 2、给消息提示框加类名（荐）
 更加推荐为这个messageBox添加一个类名，比较科学并且不会影响到其他。

```dart
// 弹出注销提示框
this.$confirm('确认注销吗?', '提示', {
  customClass: 'message-logout'
}).then(() => {
  this.$message({
    message: '已成功注销',
    type: 'success'
  })
}).catch(() => { /* 用户取消注销 */ })
...
<style scoped>
  ...
</style>
<style>
  ...
  .message-logout {
    width: 350px;
  }
</style>
```