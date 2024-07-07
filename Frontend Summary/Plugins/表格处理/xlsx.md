## XLSX

```js
# 安装依赖：首先，在你的项目中安装xlsx库。可以使用npm或者yarn进行安装。

npm install xlsx

# 导入依赖：在你的代码中导入xlsx库。

import XLSX from 'xlsx';
```

```js
# 封装Excel的读取函数：创建一个函数用于读取Excel文件，并返回读取到的数据。以下是一个简单的示例：
function readExcel(file) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onload = (e) => {
      const data = new Uint8Array(e.target.result);
      const workbook = XLSX.read(data, { type: 'array' });
      const worksheet = workbook.Sheets[workbook.SheetNames[0]];
      const jsonData = XLSX.utils.sheet_to_json(worksheet, { header: 1 });
      resolve(jsonData);
    };
    reader.onerror = (error) => reject(error);
    reader.readAsArrayBuffer(file);
  });
}
```

```js
# 封装Excel的写入函数：创建一个函数用于将数据写入Excel文件，并返回生成的Excel文件。以下是一个简单的示例：
function writeExcel(data, sheetName, fileName) {
  const worksheet = XLSX.utils.json_to_sheet(data);
  const workbook = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(workbook, worksheet, sheetName);
  const excelBuffer = XLSX.write(workbook, { bookType: 'xlsx', type: 'array' });
  const blob = new Blob([excelBuffer], { type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' });
  const url = window.URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url;
  link.download = fileName;
  link.click();
  window.URL.revokeObjectURL(url);
}
```

#### 1、引入依赖

```javascript
npm install xlsx-style --save
npm install --save xlsx
```
#### 2、运行项目会发生一个报错
This relative module was not found: ./cptable in ./node_modules/xlsx-style@0.8.13@xlsx-style/dist/cpexcel.js
#### 3、解决报错：
在\node_modules\xlsx-style\dist\cpexcel.js 807行 的
```javascript
var cpt = require('./cpt' + 'able');
```

改为：
```javascript
var cpt = cptable;
```

再次运行项目就不会报错了
#### 4、使用：
##### 4.1、引入：
```javascript
import XLSXStyle from 'xlsx-style';
```
##### 4.2、mounted使用
```javascript
this.exportExcel()
```
##### 4.3、methods定义方法
```javascript
exportExcel(){
	let data = [['时间','电压'],['2021-12-01 08:57:12','3.14'],['2021-12-01 08:58:20','3.15']];
	let titles = ['时间','电压']
	var sheet = XLSX.utils.json_to_sheet(data, {
          skipHeader: true,
        });
        /**设置标题头背景色 */
        for (const key in sheet) {
          // 第一行，表头
          if (key.replace(/[^0-9]/ig, '') === '1') {
            sheet[key].s = {
              fill: { //背景色
                fgColor: { rgb: 'C0C0C0' }
              },
              font: {//字体
                name: '宋体',
                sz: 12,
                bold: true
              },
              border: {//边框
                bottom: {
                  style: 'thin',
                  color: 'FF000000'
                }
              },
              alignment:{
                horizontal:'center' //水平居中
              }
            }
          }
          let colsP = titles.map(item=>{
            let obj = {
              'wch': 25 //列宽
            }
            return obj;
          })
          sheet['!cols']  = colsP;//列宽
        }
        this.openDownload(this.sheet2blob(sheet,titles.join('-')), titles.join('-')+"Excel文件.xlsx");
},
sheet2blob(sheet, sheetName) {
      let wb = XLSX.utils.book_new();
      wb.SheetNames.push(sheetName)
      wb.Sheets[sheetName] = sheet;
      var wbout = XLSXStyle.write(wb, { bookType: '', bookSST: false, type: 'binary' })
      var blob = new Blob([s2ab(wbout)], { type: "" },sheetName);
      // 字符串转ArrayBuffer
      function s2ab(s) {
        var buf = new ArrayBuffer(s.length);
        var view = new Uint8Array(buf);
        for (var i = 0; i != s.length; ++i) view[i] = s.charCodeAt(i) & 0xff;
        return buf;
      }
      return blob;
    },
openDownload(url, saveName) {
      if (typeof url == "object" && url instanceof Blob) {
        url = URL.createObjectURL(url); // 创建blob地址
      }
      var aLink = document.createElement("a");
      aLink.href = url;
      aLink.download = saveName || ""; // HTML5新增的属性，指定保存文件名，可以不要后缀，注意，file:///模式下不会生效
      var event;
      if (window.MouseEvent) event = new MouseEvent("click");
      else {
        event = document.createEvent("MouseEvents");
        event.initMouseEvent(
          "click",
          true,
          false,
          window,
          0,
          0,
          0,
          0,
          0,
          false,
          false,
          false,
          false,
          0,
          null
        );
      }
      aLink.dispatchEvent(event);
    },
```

### 选中数据导出excel
```javascript
import * as XLSX from 'xlsx' // vue3可用此引入
import XLSX from "xlsx"; // vue2可用此引入
```

```javascript
const workbook = XLSX.utils.book_new()
const worksheet = XLSX.utils.json_to_sheet(value)
XLSX.utils.book_append_sheet(workbook,worksheet,'shell')
XLSX.witeFile(workbook,'xxx.xlsx')
```
例子
```javascript
let arr = [
    { id: 1, sex: "男", name: "李二" },
    { id: 2, sex: "女", name: "李兰" },
  ];
 
  let aoa = [["名称", "id", "sex"]];
  for (let i = 0; i < arr.length; i++) {
    let ava = arr[i];
    aoa.push([ava.name, ava.id, ava.sex]);
  }
  //处理成表格的位置信息
  const sheet = XLSX.utils.aoa_to_sheet(aoa);
  const wb = {
    SheetNames: ["sheet1"],
    Sheets: {
      sheet1: sheet,
    },
  };
  XLSX.writeFile(wb, "航空关联列表.xlsx", {
    bookType: "xlsx",
    bookSST: false,
    type: "file",
  });

```
### 后端返回文件流下载（导出excel）
下载接口

```javascript
function xxx() {
	return axios.post('url',data,{
		responseType: 'blob'
	})
}
// 调用下载接口
xxx().then(res=>{
	saveFile(res,'xxxxx.xlsx')
})
```

在接口的成功返回中调用下面的函数就可以了。
```javascript
// blob 是后端返回的文件流
// name 文件的名字
export function saveFile (blob:Blob,name:string) {
	const dom = document.createElement('a')
	const url = window.URL.createObjectURL(blob)
	dom.herf = url
	dom.download = docodeURI(name)
	dom.style.display = 'none'
	document.body.appendChild(dom)
	dom.click()
	dom.parentNode?.removeChild(dom)
	window.URL.revokeObjectURL(url)
}
```





------



#### 1、引入依赖

```javascript
npm install xlsx-style --save
npm install --save xlsx
```
#### 2、运行项目会发生一个报错
This relative module was not found: ./cptable in ./node_modules/xlsx-style@0.8.13@xlsx-style/dist/cpexcel.js
#### 3、解决报错：
在\node_modules\xlsx-style\dist\cpexcel.js 807行 的
```javascript
var cpt = require('./cpt' + 'able');
```

改为：
```javascript
var cpt = cptable;
```

再次运行项目就不会报错了
#### 4、使用：
##### 4.1、引入：
```javascript
import XLSXStyle from 'xlsx-style';
```
##### 4.2、mounted使用
```javascript
this.exportExcel()
```
##### 4.3、methods定义方法
```javascript
exportExcel(){
	let data = [['时间','电压'],['2021-12-01 08:57:12','3.14'],['2021-12-01 08:58:20','3.15']];
	let titles = ['时间','电压']
	var sheet = XLSX.utils.json_to_sheet(data, {
          skipHeader: true,
        });
        /**设置标题头背景色 */
        for (const key in sheet) {
          // 第一行，表头
          if (key.replace(/[^0-9]/ig, '') === '1') {
            sheet[key].s = {
              fill: { //背景色
                fgColor: { rgb: 'C0C0C0' }
              },
              font: {//字体
                name: '宋体',
                sz: 12,
                bold: true
              },
              border: {//边框
                bottom: {
                  style: 'thin',
                  color: 'FF000000'
                }
              },
              alignment:{
                horizontal:'center' //水平居中
              }
            }
          }
          let colsP = titles.map(item=>{
            let obj = {
              'wch': 25 //列宽
            }
            return obj;
          })
          sheet['!cols']  = colsP;//列宽
        }
        this.openDownload(this.sheet2blob(sheet,titles.join('-')), titles.join('-')+"Excel文件.xlsx");
},
sheet2blob(sheet, sheetName) {
      let wb = XLSX.utils.book_new();
      wb.SheetNames.push(sheetName)
      wb.Sheets[sheetName] = sheet;
      var wbout = XLSXStyle.write(wb, { bookType: '', bookSST: false, type: 'binary' })
      var blob = new Blob([s2ab(wbout)], { type: "" },sheetName);
      // 字符串转ArrayBuffer
      function s2ab(s) {
        var buf = new ArrayBuffer(s.length);
        var view = new Uint8Array(buf);
        for (var i = 0; i != s.length; ++i) view[i] = s.charCodeAt(i) & 0xff;
        return buf;
      }
      return blob;
    },
openDownload(url, saveName) {
      if (typeof url == "object" && url instanceof Blob) {
        url = URL.createObjectURL(url); // 创建blob地址
      }
      var aLink = document.createElement("a");
      aLink.href = url;
      aLink.download = saveName || ""; // HTML5新增的属性，指定保存文件名，可以不要后缀，注意，file:///模式下不会生效
      var event;
      if (window.MouseEvent) event = new MouseEvent("click");
      else {
        event = document.createEvent("MouseEvents");
        event.initMouseEvent(
          "click",
          true,
          false,
          window,
          0,
          0,
          0,
          0,
          0,
          false,
          false,
          false,
          false,
          0,
          null
        );
      }
      aLink.dispatchEvent(event);
    },
```

# vue3中如何使用xlsx插件

- - 

## 1. vue3中如何使用xlsx插件

在Vue3中使用`xlsx`插件来实现Excel的上传与下载，你需要经历几个关键步骤。

下面是一个简化的指南，展示如何实现这些功能：

### 1.1. 安装`xlsx`库

首先，确保你已经安装了`xlsx`库。如果还没有安装，可以通过以下命令进行安装：

```
npm install xlsx
```

### 1.2. 导入`xlsx`库

在你想要使用`xlsx`功能的Vue组件中，导入`xlsx`库：

```
<script setup>
import * as XLSX from 'xlsx';
</script>
```

### 1.3. 实现Excel上传

#### 1.3.1. **创建文件输入组件**：

在模板中添加一个文件输入框，用于用户选择Excel文件。

#### 1.3.2. **处理文件读取**：

在`onFileChange`方法中，使用`FileReader`读取用户选择的文件，并利用`xlsx`进行解析。

```js
<template>
  <div>
    <input type="file" @change="onFileChange" accept=".xlsx, .xls" />
    <!-- 展示解析后的数据 -->
  </div>
</template>

<script setup>
import * as XLSX from 'xlsx';

const onFileChange = (event) => {
  const file = event.target.files[0];
  if (!file) return;

  const reader = new FileReader();
  reader.onload = (e) => {
    const data = e.target.result;
    const workbook = XLSX.read(data, { type: 'binary' });
    const sheetName = workbook.SheetNames[0];
    const worksheet = workbook.Sheets[sheetName];
    const excelData = XLSX.utils.sheet_to_json(worksheet);
    // 处理excelData，如存入Vue的响应式数据中
  };
  reader.readAsBinaryString(file);
};
</script>
```

### 1.4. 实现Excel下载

#### 1.4.1. **定义导出方法**：

创建一个方法来将数据转换为Excel格式并提供下载。

```js
const downloadExcel = (data, filename = 'export.xlsx') => {
  const worksheet = XLSX.utils.json_to_sheet(data);
  const workbook = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(workbook, worksheet, 'Sheet1');

  // 将工作簿转换为二进制字符串
  const wbout = XLSX.write(workbook, { bookType: 'xlsx', type: 'binary' });

  function s2ab(s) {
    const buf = new ArrayBuffer(s.length);
    const view = new Uint8Array(buf);
    for (let i = 0; i !== s.length; ++i) view[i] = s.charCodeAt(i) & 0xFF;
    return buf;
  }

  // 创建Blob对象并触发下载
  const blob = new Blob([s2ab(wbout)], { type: '' });
  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url;
  link.download = filename;
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};
```

#### 1.4.2. **使用导出方法**：

在适当的地方调用`downloadExcel`方法，传入你希望导出的数据。

```
<button @click="downloadExcel(yourData)">导出Excel</button>
```

在这个例子中，`yourData`是你要导出的数据数组。

这样，你就可以在Vue3应用中实现Excel文件的上传与下载功能了。

记得根据实际情况调整代码，比如处理错误、优化用户体验等。

