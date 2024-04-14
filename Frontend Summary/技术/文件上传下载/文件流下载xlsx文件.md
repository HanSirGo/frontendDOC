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

> 用户将附件文件上传之后，返回的是一个链接地址；当另一个用户需要查看这份附件的时候，就需要点击链接地址下载下来

```js
const download = () => {
    const x = new window.XMLHttpRequest();
    x.open('GET', 'http://192.168.1.1/abcd.xlsx', true);
    x.responseType = 'blob';
    x.onload = () => {
        const url = window.URL.createObjectURL(x.response);
        const a = document.createElement('a');
        a.href = url;
        a.download = 'file.xlsx';
        a.click();
    };
    x.send();
}
```

