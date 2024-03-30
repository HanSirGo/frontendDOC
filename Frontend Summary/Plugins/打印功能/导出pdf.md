## 将html页面下载成pdf文件

> html2canvas：将html页面转换成图片
>
> jspdf：将图片转成pdf文件导出

```html
<div id="qrCode" class="qrCode">
    <!-- 此div中放需要导出成pdf的html内容 -->
    <div style="height: 50px;text-align: center;">标题</div>
    <div style="height: 400px;text-align: center;">内容</div>
</div>
```

```js
getImg() {
    let content = document.getElementById('qrCode')
    html2canvas(content).then(canvas => {
        let contentWidth = canvas.width
        let contentHeight = canvas.height
        let imgWidth = 595.28
        let imgHeight = 592.28 / contentWidth * contentHeight
        let pageData = canvas.toDataURL('image/jpeg', 1.0)
        // let PDF = new JsPDF('l', 'pt', 'a4');//导出竖版
        let PDF = new JsPDF('p', 'pt', 'a4');//导出横版
        /*  new jsPDF()传三个参数 */
        PDF.addImage(pageData, 'JPEG', 0, 0, imgWidth, imgHeight)
        PDF.save('文档.pdf')
    })
}
```

> **new jsPDF()传三个参数**
>
> 第一个参数：l：横向 p：纵向
>
> 第二个参数：测量单位（"pt"，"mm", "cm", "m", "in" or "px"）
>
> 第三个参数：格式，默认为“a4”
>
> 
>
> **PDF.addImage()传九个参数**
>
> pdf.addImage(image, format, x, y, width, height, alias, compression, rotation)
>
> 具体参数
>
> 1、image：表示要插入的图片资源，可以是图片文件的路径或者base64编码字符串。
>
> 2、format：表示要插入的图片格式，包括：‘JPEG’, ‘PNG’, ‘GIF’, ‘BMP’, ‘TIFF’, ‘RAW’, ‘JPEG2000’。
>
> 3、x：图片在PDF中的x轴坐标，单位为pt（点）。
>
> 4、y：图片在PDF中的y轴坐标，单位为pt（点）。
>
> 5、width：图片在PDF中的宽度，单位为pt（点）。
>
> 6、height：图片在PDF中的高度，单位为pt（点）。
>
> 7、alias（可选）：指定图片资源的别名。
>
> 8、compression（可选）：指定图片的压缩质量，取值为0-1之间的浮点数。
>
> 9、rotation（可选）：指定图片的旋转角度，取值范围为0-360之间的整数。

