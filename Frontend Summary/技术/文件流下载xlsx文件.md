## 后端返回文件流,前端下载xlsx文件

> 适用场景：后端返回xlsx文件流，前端进行处理并下载。

```js
 saveFile(res,filename){
     //文件流，文件名
     try{
         // 处理文件流
         const blob = new Blob([res])
         if(res.type.indexOf("application/json")>-1){
             var reader = new FileReader()
             reader.readAsText(blob)
             reader.onload = e => {
                 console.log(e.target.result);
             }
             return
         }
         const fileName = filename + ".xlsx"
         if("download" in document.createElement('a')){
             //非IE下载
             const elink = document.createElement("a")
             elink.download = fileName
             elink.style.display = "none"
             elink.href = URL.createObjectURL(blob)
             document.body.appendChild(elink)
             elink.click()
             URL.revokeObjectURL(elink.href)
             document.body.removeChild(elink)
         }else{
             //IE10+下载
             navigator.msSaveBlob(blob,fileName)
         }
     }catch{

     }
 }
```

