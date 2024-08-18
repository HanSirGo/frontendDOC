# 探索Node.js中的node-xlsx：将Excel文件解析为JSON

在Node.js开发中，处理Excel文件是一个常见需求，特别是在需要导入大量数据或生成报表的场景中。`node-xlsx` 是一个强大的库，它提供了Excel文件的解析和生成功能。本文将深入探讨 `node-xlsx` 的使用，并通过一个案例演示如何将Excel文件解析为JSON格式。

#### **安装node-xlsx**

首先，你需要在你的Node.js项目中安装 `node-xlsx`。打开终端或命令行工具，定位到你的项目目录，然后运行以下命令：

```
npm install node-xlsx
```

#### **node-xlsx的基本使用**

`node-xlsx` 提供了两个主要功能：解析Excel文件和构建Excel文件。解析功能通过 `xlsx.parse()` 方法实现，而构建功能则通过 `xlsx.build()` 方法实现。

##### **解析Excel文件**

当你需要读取Excel文件并将其内容转换为JSON格式时，可以使用 `xlsx.parse()` 方法。这个方法接受文件路径作为参数，并返回一个包含所有工作表数据的对象数组。

##### **构建Excel文件**

如果你需要将数据导出为Excel文件，`xlsx.build()` 方法是最佳选择。这个方法接受一个包含工作表数据的数组，并生成一个可以写入文件的Buffer。

#### **案例：将Excel文件解析为JSON**

接下来，我们将通过一个具体的案例来演示如何使用 `node-xlsx` 将Excel文件解析为JSON格式。

##### **场景描述**

假设你有一个名为 `origin.xlsx` 的Excel文件，里面包含了企业数据。这个文件包含多个工作表，但我们现在只关注第一个工作表。每个工作表都有标题行，随后的行是具体的数据。

##### **编写代码**

首先，确保你的项目中已经安装了 `node-xlsx`。然后，创建一个新的JavaScript文件（例如 `xlsxDeal.js`），

![1721465141282](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721465141282.png)

并编写以下代码：

```js
const nodeXlsx = require('node-xlsx');
const fs = require('fs');
// 读取xlsx
const workbook = nodeXlsx.parse('./origin.xlsx');
let excelContent = workbook[0].data;

let resultList = [];

let titleList = excelContent[0];
let contentList = excelContent.slice(1);

contentList.forEach((v, i) => {
  let obj = {};
  v.forEach((v2, i2) => {
    obj[titleList[i2]] = v2;
  });

  resultList.push(obj);
});

fs.writeFileSync('./resultJSON.json', JSON.stringify(resultList, null, 2), 'utf-8');

console.log('Excel文件已成功解析为JSON并保存到文件。');
```

##### **执行代码**

保存文件后，在命令行中运行你的脚本：

```
node xlsxDeal.js
```

执行成功后，你将在同一目录下找到一个名为 `resultJSON.json` 的文件，里面包含了从 `origin.xlsx` 文件中解析出的数据。

![1721465216395](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721465216395.png)

#### **总结**

`node-xlsx` 是一个功能强大的库，它使得在Node.js中处理Excel文件变得简单而高效。通过上面的案例，我们展示了如何使用 `node-xlsx` 将Excel文件解析为JSON格式，这对于数据导入和处理非常有用。你可以根据自己的需求，进一步探索 `node-xlsx` 的其他功能，如设置列宽、处理日期格式等。

