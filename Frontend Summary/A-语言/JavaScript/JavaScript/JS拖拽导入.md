## JS拖拽导入

### 清空文件记录

```
# 在js中,拖拽导入文件会触发一系列的事件,包括 dragenter、dragover、drop等,当文件被拖拽到页面上并放下(即drop事件发生)时,通常会创建一个包含文件信息的DataTransfer对象.

# 如果你想在每次拖拽操作后清空文件记录,你可以在drop事件处理函数中删除或重置相关的数据.

document.addEventListener('drop',(event)=>{
	// 阻止默认行为
	event.preventDefault()
	
	// 获取文件列表
	cont files = event.dataTransfer.files
	
	// 清空文件列表
	event.dataTransfer.clearData()
})
```

