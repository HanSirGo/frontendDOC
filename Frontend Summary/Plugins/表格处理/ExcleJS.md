## ExcleJS

前端导入导出excel文件时使用的是**xlsx这个库**。但是，如果想要修改excel表格样式的话，是需要使用收费的专业版本。带着开源第一，绝不花钱的基本原则，就找到了ExcleJS这个库。

### 具体实现

安装：

```js
npm install exceljs
npm install file-saver
```

1.创建workbook，添加名为Demo的sheet，设置默认行高为20，设置列（表头）；添加行信息（allData前端页面表格中的数据）；最后给表头添加颜色。

```js
  // 创建工作簿
    const workbook = new ExcelJs.Workbook();
    // 添加sheet
    const worksheet = workbook.addWorksheet('Demo');
    // 设置 sheet 的默认行高
    worksheet.properties.defaultRowHeight = 20;
    // 设置列
    worksheet.columns = [
      { header: 'Index', key: 'index', width: 10 },
      { header: 'Name', key: 'name', width: 25 },
      { header: 'Type', key: 'group', width: 25, outlineLevel: 1 },
      { header: 'Group', key: 'type', width: 25, outlineLevel: 1 },
      { header: 'Region', key: 'modbusRegion', width: 25, outlineLevel: 1 },
      { header: 'Address', key: 'modbusAddress', width: 25, outlineLevel: 1 },
    ];
    
    // 添加行
    worksheet.addRows(allData);

    // 给表头添加背景色
    let headerRow = worksheet.getRow(1);
    headerRow.eachCell((cell) => {
      cell.fill = {
        type: 'pattern',
        pattern: 'solid',
        fgColor: {argb: 'dde0e7'},
      }
    })
```

2. 将自动筛选器设置为从 A2 到 F1 （Group、Type、Region）

```js
 // 将自动筛选器设置为从 A2 到 F1
    worksheet.autoFilter = {
      from: 'C1',
      to: 'E1',
    }
```

3. 锁定整个excel表格，可筛选但不能选中锁定的单元格

```js
// 锁定工资表
    await worksheet.protect('the-password', 
                             {
                              autoFilter:true,
                            selectLockedCells:false,
                              });
```

4. 通过循环判断，哪些单元格可以被用户操作，并且判断该单元格的输入限制是什么

```js
const allData = [...this.state.dataSource]
   let length = allData.length

   for(let i = 0 ;i < length; i++){
      // Region的输入范围
      let coilArr = ['"Coil,Discrete Input"']
      let registerArr = ['"Holding Register,Input Register"']
      let listArr = []

      if(allData[i].type === 'BOOL'){
        listArr = coilArr
      }
      else{
        listArr = registerArr
      }
      // 可编辑的单元格在E、F中
      worksheet.getCell(`E${i+2}`).protection = {
        locked: false,
      };

      // Region的输入校验
      worksheet.getCell(`E${i+2}`).dataValidation = {
        type: 'list',
        allowBlank: true,
        formulae: listArr,
        showErrorMessage: true,
        errorTitle: '非法输入',
        error: '取值范围为:'+listArr

      };
      worksheet.getCell(`F${i+2}`).protection = {
        locked: false,
      };

      // Address的输入校验
      worksheet.getCell(`F${i+2}`).dataValidation = {
        type: 'whole',
        operator: 'between',
        allowBlank: true,
        showErrorMessage: true,
        formulae: [0,65535],
        errorTitle: '非法输入',
        error: '取值范围为:[0,65535]'
      };

    }
```

以上的代码中，worksheet.getCell(`E${i+2}`).dataValidation是进行单元格数据验证的函数，具体的使用可参考**官方文档**[3]。

5. 导出名为"xlsx-demo.xlsx"的excel文件

```js
    // 导出excel    
    this.saveWorkbook(workbook, 'xlsx-demo.xlsx');
```

### 整个函数展示

```js
整个函数展示
  // 导出xls
  exportXLS = async () =>{

    const allData = [...this.state.dataSource]
    let length = allData.length
    // 创建工作簿
    const workbook = new ExcelJs.Workbook();
    // 添加sheet
    const worksheet = workbook.addWorksheet('demo');
    // 设置 sheet 的默认行高
    worksheet.properties.defaultRowHeight = 20;
    // 设置列
    worksheet.columns = [
      { header: 'Index', key: 'index', width: 10 },
      { header: 'Name', key: 'name', width: 25 },
      { header: 'Group', key: 'group', width: 25, outlineLevel: 1 },
      { header: 'Type', key: 'type', width: 25, outlineLevel: 1 },
      { header: 'Region', key: 'modbusRegion', width: 25, outlineLevel: 1 },
      { header: 'Address', key: 'modbusAddress', width: 25, outlineLevel: 1 },
    ];
    
    // 添加行
    worksheet.addRows(allData);
    // 给表头添加背景色
    let headerRow = worksheet.getRow(1);
    // 通过 cell 设置背景色，更精准
    headerRow.eachCell((cell) => {
      cell.fill = {
        type: 'pattern',
        pattern: 'solid',
        fgColor: {argb: 'dde0e7'},
      }
    })
    // 将自动筛选器设置为从 C1 到 F1
    worksheet.autoFilter = {
      from: 'C1',
      to: 'E1',
    }

    // 锁定工资表
    await worksheet.protect('the-password', 
                             {
                              autoFilter:true,
                              selectLockedCells:false,
                              });

    // 判断哪些单元格可以被用户操作，并且判断该单元格的输入限制是什么
    for(let i = 0 ;i < length; i++){
      // 根据不同类型选择筛选的框
      let coilArr = ['"Coil,Discrete Input"']
      let registerArr = ['"Holding Register,Input Register"']
      let listArr = []
      if(allData[i].type === 'BOOL'){
        listArr = coilArr
      }
      else{
        listArr = registerArr
      }

      // 可编辑的单元格在E、F中
      worksheet.getCell(`E${i+2}`).protection = {
        locked: false,
      };

      worksheet.getCell(`E${i+2}`).dataValidation = {
        type: 'list',
        allowBlank: true,
        formulae: listArr,
        showErrorMessage: true,
        errorTitle: '非法输入',
        error: '取值范围为:'+listArr

      };

      worksheet.getCell(`F${i+2}`).protection = {
        locked: false,
      };

      worksheet.getCell(`F${i+2}`).dataValidation = {
        type: 'whole',
        operator: 'between',
        allowBlank: true,
        showErrorMessage: true,
        formulae: [0,65535],
        errorTitle: '非法输入',
        error: '取值范围为:[0,65535]'

      };

    }

    // 导出excel
    this.saveWorkbook(workbook, 'xlsx-demo.xlsx');
    
  }
```

