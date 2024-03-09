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


