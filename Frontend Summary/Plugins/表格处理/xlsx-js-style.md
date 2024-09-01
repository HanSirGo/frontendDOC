## xlsx-js-style

可以在导出excel数据时，修改表格样式，xlsx插件没有此功能。

```js
import XLSX from 'xlsx-js-style'

// 创建工作簿
const sheet = XLSX.utils.json_to_sheet(data)

console.log(sheet)
// {
//    !ref:"A1:D2",
//    A1: { t:"s", v:"_id" },
//    A2: { t:"s", v:"DealObject.ReferenceData.1" },
//    B1: { t:"s", v:"systemName" },
//    B2: { t:"s", v:"DealObject" },
//    C1: { t:"s", v:"objectName" },
//    C2: { t:"s", v:"Deal" },
//    D1: { t:"s", v:"logicId" },
//    D2: { t:"s", v:"1" },
// }

// 添加样式，文字统一为 等线 11号字
Object.keys(sheet).forEach(item=>{
    if(item!==='!ref') {
        sheet[item].s = {
            font:{sz:11, name:'等线'}
        }
    }
})
// {
//    !ref:"A1:D2",
//    A1: { t:"s", v:"_id", s:{ font:{sz:11, name:'等线'} } },
//    A2: { t:"s", v:"DealObject.ReferenceData.1", s:{ font:{sz:11, name:'等线'} } },
//    B1: { t:"s", v:"systemName", s:{ font:{sz:11, name:'等线'} } },
//    B2: { t:"s", v:"DealObject", s:{ font:{sz:11, name:'等线'} } },
//    C1: { t:"s", v:"objectName", s:{ font:{sz:11, name:'等线'} } },
//    C2: { t:"s", v:"Deal", s:{ font:{sz:11, name:'等线'} } },
//    D1: { t:"s", v:"logicId", s:{ font:{sz:11, name:'等线'} } },
//    D2: { t:"s", v:"1", s:{ font:{sz:11, name:'等线'} } },
// }

// 创建工作簿
const book = XLSX.utils.book_new()
// 将工作表添加到工作簿
XLSX.utils.book_append_sheet(book, sheet, 'sheet1')
// 生成Excel的配置项
// XLSX.write(sheet, { bookType: 'xlsx', type: 'binary' })
XLSX.writeFile(book, filename)
```

```js
// 给表格的某一个单元格单独添加样式

const sheet = XLSX.utils.json_to_sheet(data)

sheet["A1"].s = {
    font: { // 设置字体样式
        bold: true，
        sz:14,
        color: { rgb:"FFFF00" }
    },
    alignment: {
        horizontal: "center" // 水平居中
    },
    fill: {
        fgColor: { rgb: "FFFF00" } // 设置背景色为黄色
    }
}
```

