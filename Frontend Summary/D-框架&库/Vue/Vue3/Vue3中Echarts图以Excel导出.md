# Vue3中Echarts图以Excel导出

![1713683058037](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713683058037.png)

# **说明**

我们要把上变的Echarts树状图要以Excel的形式导出，导出来的结果大概就是以下样子：

![1713683074308](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713683074308.png)

![1713683086595](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713683086595.png)

# **安装工具依赖**

在我们项目中执行以下命令安装所需依赖：

```js
npm install exceljs file-saver --save
// exceljs包具体使用：https://github.com/exceljs/exceljs/blob/master/README_zh.md
```

# **项目使用**

先添加以下导出图标的按钮

```vue
<template>
  <el-button class="export-excel" @click="exportExcel"> 导出图表</el-button>
  <div id="chart" ref="changeChart"></div>
</template>
```

![1713683113779](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713683113779.png)

```js
<script lang="ts" setup>
import * as echarts from 'echarts'
import { changeZoneOption } from './statisticAnalysisEcharts' // echarts的options
import { Excel } from './exportExcel' // 封装的excel导出工具
const changeChart = ref<any>(null)
let areaChart: any

// 导出excel
function exportExcel() {
  const excel = new Excel()
  ....
  ...
}
onMounted(() => {
  areaChart = echarts.init(changeChart.value)
  changeZoneOption && areaChart.setOption(changeZoneOption)
})
</script>
```

我们主要实现exportExcel封装的工具，在实现之前我们需要看一下exportExcel函数做了哪些事儿：

1. 获取图表实例生成数据（也可以直接从后端获取我们想要的数据格式）

我们导出的excel有两页一页是数据表格，一页是是数据图片表，所以我们需要获得数据还有生成图片

```js
// 导出excel
function exportExcel() {
  const chart: any = echarts.getInstanceByDom(changeChart.value) // 获取图表实例
  const option = chart.getOption() // 获取到的option进行数据改造，这里不再展示

  const base64Image = chart.getDataURL({
    pixelRatio: 2, // 导出图片的分辨率比例，默认为1，即图片的分辨率为屏幕分辨率的一倍
    backgroundColor: '#fff', // 导出图片的背景色
  })
  
  const excel = new Excel()
    excel.exportExcel([
    {
      type: '数据表',
      name: '表格文件名称',
      title: '表格标题',
      header: ['月份', '2022', '2023', '2024'],
      data: [
        ['6月', 1, 2, 0],
        ['7月', 3, 2, 0],
        ['8月', 1, 2, 0],
        ['9月', 3, 2, 0],
        ['10月', 1, 2, 0],
        ['11月', 3, 2, 0],
        ['12月', 1, 2, 0],
      ],
      customHeader: ['月份', '2022', '2023', '2024'],
      sheetName: '数据表',
    },
    {
      type: '图片表',
      sheetName: '图片表',
      base64Image: base64Image,
    },
  ])
  ....
  ...
}
```

2. 实现封装Excel工具类 创建exportExcel.ts文件

```js
/**
 * 封装excel.ts工具文件
 */
import ExcelJS from 'exceljs'
import { saveAs } from 'file-saver' // 引入file-saver, 用于保存文件


// 合并单元格数据类型
export interface mergeListType {
  startRow: number
  startColumn: number
  endRow: number
  endColumn: number
}
export interface exportExcelType {
  type: '数据表'
  // 数据
  data: { [key: string]: any }[]
  // 文件名
  name: string
  // 表头字段
  header: string[]
  // 表头字段对应中文
  customHeader: string[]
  // 工作表名
  sheetName?: string
  // 标题
  title?: string
  // 小标题
  subTitle?: string
  // 工作表保护密码
  password?: string
  // 对齐方式
  alignment?: Partial<ExcelJS.Alignment>
  // 合并单元格
  mergeList?: mergeListType[]
  // 标题样式
  titleStyle?: Partial<ExcelJS.Row>
  // 小标题样式
  subTitleStyle?: Partial<ExcelJS.Row>
  // 表头样式
  headerStyle?: Partial<ExcelJS.Row>
  // 单元格统一样式
  cellStyle?: Partial<ExcelJS.Row & ExcelJS.Column>
}
export interface excelImageType {
  type: '图片表'
  // 数据
  base64Image: any
  sheetName?: string
}

export class Excel {
  worksheet?: ExcelJS.Worksheet // 当前工作表
  header: string[] // 表头字段数组
  workbook: ExcelJS.Workbook // 工作簿对象
  constructor() {
    this.worksheet = undefined
    this.header = []
    this.workbook = new ExcelJS.Workbook()
    this.workbook.creator = '作者' // excel属性作者
    this.workbook.created = new Date() // excel属性创建时间
  }
  ...
  ...
  ...
}
```

3. 实现exportExcel方法 我们看到exportExcel方法传入了一个数组，包含了第一页的数据页和第二页的图片表页

```js
 public async exportExcel(optionsArray: [exportExcelType, excelImageType]): Promise<void> {
    const promisAll: any[] = []
    optionsArray.forEach((item: exportExcelType | excelImageType) => {
      if (item.type === '数据表') {
        promisAll.push(this.excelOption(item)) // 生成数据表的方法
      } else if (item.type === '图片表') {
        promisAll.push(this.excelImage(item)) // 生成图片表的方法
      }
    })
    
    Promise.all(promisAll).then(() => {
      // 导出excel
      this.workbook.xlsx.writeBuffer().then((buffer) => {
        // 生成excel文件的二进制数据
        saveAs.saveAs(
          new Blob([buffer], {
            // 生成Blob对象
            type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet',
          }),
          '文件名字' + '.xlsx',
        ) // 指定文件名
      })
    })
  }
```

4. 实现excelOption生成数据表的方法 该方法需要对数据Excel进行处理，最基本的插入单元格数据，插入大标题等

```js
 public async exportExcel(optionsArray: [exportExcelType, excelImageType]): Promise<void> {
    const promisAll: any[] = []
    optionsArray.forEach((item: exportExcelType | excelImageType) => {
      if (item.type === '数据表') {
        promisAll.push(this.excelOption(item)) // 生成数据表的方法
      } else if (item.type === '图片表') {
        promisAll.push(this.excelImage(item)) // 生成图片表的方法
      }
    })
    
    Promise.all(promisAll).then(() => {
      // 导出excel
      this.workbook.xlsx.writeBuffer().then((buffer) => {
        // 生成excel文件的二进制数据
        saveAs.saveAs(
          new Blob([buffer], {
            // 生成Blob对象
            type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet',
          }),
          '文件名字' + '.xlsx',
        ) // 指定文件名
      })
    })
  }
```

4. 实现excelImage生成图片表的方法

```js
 private async excelImage(options: excelImageType): Promise<void> {
    const { sheetName = 'sheet1', base64Image } = options

    const worksheet = this.workbook.addWorksheet(sheetName)
    const image = this.workbook.addImage({
      // 添加图片
      base64: base64Image, // 图片的base64编码
      extension: 'png', // 图片的扩展名
    })
    worksheet.addImage(image, 'A1:J20') // 将图片添加到工作表中
  }
```

5. 完整代码

```js
/**
 * 封装excel.ts工具文件
 */
import ExcelJS from 'exceljs'
import { saveAs } from 'file-saver' // 引入file-saver, 用于保存文件

// 合并单元格数据类型
export interface mergeListType {
  startRow: number
  startColumn: number
  endRow: number
  endColumn: number
}
export interface exportExcelType {
  type: '数据表'
  // 数据
  data: { [key: string]: any }[]
  // 文件名
  name: string
  // 表头字段
  header: string[]
  // 表头字段对应中文
  customHeader: string[]
  // 工作表名
  sheetName?: string
  // 标题
  title?: string
  // 小标题
  subTitle?: string
  // 工作表保护密码
  password?: string
  // 对齐方式
  alignment?: Partial<ExcelJS.Alignment>
  // 合并单元格
  mergeList?: mergeListType[]
  // 标题样式
  titleStyle?: Partial<ExcelJS.Row>
  // 小标题样式
  subTitleStyle?: Partial<ExcelJS.Row>
  // 表头样式
  headerStyle?: Partial<ExcelJS.Row>
  // 单元格统一样式
  cellStyle?: Partial<ExcelJS.Row & ExcelJS.Column>
}
export interface excelImageType {
  type: '图片表'
  // 数据
  base64Image: any
  sheetName?: string
}
export class Excel {
  worksheet?: ExcelJS.Worksheet // 当前工作表
  header: string[] // 表头字段数组
  workbook: ExcelJS.Workbook // 工作簿对象
  constructor() {
    this.worksheet = undefined
    this.header = []
    this.workbook = new ExcelJS.Workbook()
    this.workbook.creator = '作者' // excel属性作者
    this.workbook.created = new Date() // excel属性创建时间
  }
  public async exportExcel(optionsArray: [exportExcelType, excelImageType]): Promise<void> {
    const promisAll: any[] = []
    optionsArray.forEach((item: exportExcelType | excelImageType) => {
      if (item.type === '数据表') {
        promisAll.push(this.excelOption(item))
      } else if (item.type === '图片表') {
        promisAll.push(this.excelImage(item))
      }
    })
    Promise.all(promisAll).then(() => {
      // 导出excel
      this.workbook.xlsx.writeBuffer().then((buffer) => {
        // 生成excel文件的二进制数据
        saveAs.saveAs(
          new Blob([buffer], {
            // 生成Blob对象
            type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet',
          }),
          'name' + '.xlsx',
        ) // 指定文件名
      })
    })
  }
  private async excelImage(options: excelImageType): Promise<void> {
    const { sheetName = 'sheet1', base64Image } = options

    const worksheet = this.workbook.addWorksheet(sheetName)
    const image = this.workbook.addImage({
      // 添加图片
      base64: base64Image, // 图片的base64编码
      extension: 'png', // 图片的扩展名
    })
    worksheet.addImage(image, 'A1:J20') // 将图片添加到工作表中
  }
  private async excelOption(options: exportExcelType): Promise<void> {
    const { data, header, customHeader, sheetName = 'sheet1', title = '', subTitle = '', password = '', mergeList = [], titleStyle, subTitleStyle, headerStyle, cellStyle } = options
    // 添加sheet
    this.worksheet = this.workbook.addWorksheet(sheetName)
    this.header = header
    // 表头行序号
    let headerRowId = 1
    if (title) {
      headerRowId++
    }
    if (subTitle) {
      headerRowId++
    }
    // 插入单元格数据
    this.setCells(data, customHeader, cellStyle)
    // 插入大标题
    this.getTitle(title, titleStyle)
    // 插入小标题
    this.getSubTitle(subTitle, subTitleStyle)
    // 处理表头
    this.setHeaderStyle(headerRowId, data, headerStyle)
    // 更多处理
    this.handleDealExcel(password, headerRowId, mergeList)
  }

  // 插入单元格数据
  private setCells(data: exportExcelType['data'], customHeader: string[], style?: Partial<ExcelJS.Row & ExcelJS.Column>): void {
    // 设置列，插入中文表头
    const column: Partial<ExcelJS.Column>[] = []
    this.header?.forEach((item, index) => {
      column.push({
        header: customHeader[index],
        key: item,
        width: style?.width || 25,
      })
    })
    this.worksheet!.columns = column
    // 设置行，插入数据
    this.worksheet?.addRows(data)
    // 设置行高
    this.worksheet?.eachRow({ includeEmpty: true }, (row) => {
      row.height = style?.height || 20
    })
    // 获取每一列数据，再依次对齐
    this.worksheet!.columns.forEach((column) => {
      column.alignment = style?.alignment || { vertical: 'middle', horizontal: 'center', wrapText: true }
    })
  }

  // 插入大标题
  private getTitle(title: string, style?: Partial<ExcelJS.Row>): void {
    if (title) {
      // 插入标题
      this.worksheet?.spliceRows(1, 0, [title])
      this.worksheet?.mergeCells(1, 1, 1, this.header?.length)

      // 调正标题样式
      const titleRow = this.worksheet!.getRow(1)
      // 高度
      titleRow.height = style?.height || 40
      // 字体
      titleRow.font = style?.font || {
        size: 20,
        bold: true,
      }
      // 背景色
      titleRow.fill = style?.fill || {
        bgColor: {
          argb: 'FFFFFF00',
        },
        type: 'pattern',
        pattern: 'none',
      }
      // 对齐方式
      titleRow.alignment = style?.alignment || {
        horizontal: 'center',
        vertical: 'middle',
      }
    }
  }

  // 插入小标题
  private getSubTitle(subTitle: string, style?: Partial<ExcelJS.Row>): void {
    if (subTitle) {
      this.worksheet?.spliceRows(2, 0, [subTitle])
      this.worksheet?.mergeCells(2, 1, 2, this.header.length)
      // 调整标题样式
      const subtitleRow = this.worksheet!.getRow(2)
      // 高度
      subtitleRow.height = style?.height || 20
      // 字体设置
      subtitleRow.font = style?.font || {
        size: 14,
      }
      // 背景色
      subtitleRow.fill = style?.fill || {
        bgColor: {
          argb: 'FFFFFF00',
        },
        type: 'pattern',
        pattern: 'none',
      }
      // 对齐方式
      subtitleRow.alignment = style?.alignment || {
        horizontal: 'right',
        vertical: 'middle',
      }
    }
  }

  // 表头样式
  private setHeaderStyle(num: number,  data: exportExcelType['data'], style?: Partial<ExcelJS.Row>, data:): void {
    // 自动筛选器
    this.worksheet!.autoFilter = {
      from: {
        row: num,
        column: 1,
      },
      to: {
        row: data.length,
        column: this.header.length,
      },
    }
    // 给表头添加背景色
    const headerRow = this.worksheet!.getRow(num)
    headerRow!.height = style?.height || 30
    // 通过 cell 设置背景色，更精准
    headerRow?.eachCell((cell) => {
      cell.fill = style?.fill || {
        type: 'pattern',
        pattern: 'solid',
        fgColor: { argb: 'dde0e7' },
      }
      cell.font = style?.font || {
        size: 12,
      }
    })
  }

  // 处理excel
  private handleDealExcel(password: string, headerRowId: number, mergeList?: mergeListType[]) {
    // 添加工作表保护
    if (password) {
      this.worksheet?.protect(password, {
        autoFilter: true,
        selectLockedCells: false,
      })
    }
    // 合并单元格
    mergeList?.forEach((item) => {
      // 行数为表格数据所在行行数+表头行序号headerRowId
      const startRow = item.startRow + headerRowId
      const endRow = item.endRow + headerRowId
      this.worksheet?.mergeCells(startRow, item.startColumn, endRow, item.endColumn)
    })
    // 冻结前几行
    this.worksheet!.views = [{ state: 'frozen', xSplit: 0, ySplit: headerRowId }]
  }
}
```