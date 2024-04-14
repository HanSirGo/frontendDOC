## Canvas封装图片压缩功能

## 整体思路

1. 创建input框实现图片上传
2. 将上传的文件转成base64格式
3. 前端通过base64进行原始图片展示，并将此图片进行压缩
4. 将压缩后的图片传给服务器

## 代码实现

首先我们要实现input上传文件并做一些细节处理，细节处理要对上传的文件类型和上传的文件大小做限制，类型和大小这里我们可以根据规则自定义。

```html
<div class="container">
    <input type="file" id="upload" />
</div>
```

```js
const ACCEPT = ['image/jpg', 'image/png', 'image/jpeg'] // 所能接受的类型（自定义）
const MAXSIZE = 3 * 1024 * 1024 // 上传图片大小限制（自定义）
const MAXSIZE_STR = '3MB'
const upload = document.getElementById('upload')
upload.addEventListener('change', (e) => {
    const [file] = e.target.files
    if (!file) return // 上传文件为空时终止
    const { type: fileType, size: fileSize } = file
    // 类型判断
    if (!ACCEPT.includes(fileType)) {
        alert('不支持' + fileType + '文件类型')
        upload.value = ''
        return
    }
    // 图片大小判断
    if (fileSize > MAXSIZE) {
        alert(`文件超出${MAXSIZE_STR}限制`)
        upload.value = ''
        return
    }
    // 压缩图片
    covertImageToBase64(file, (base64Image) =>
        compress(base64Image, uploadServer)
    )
})
```

当所有限制都通过之后，就要将图片转为base64格式，也就是进入到**covertImageToBase64**函数，我们来看看这个函数做了什么

```js
function covertImageToBase64(file, callback) {
    let reader = new FileReader()
    reader.readAsDataURL(file) // 读取二进制数据，并将其编码为 base64 的 data url

    // 读取完成进行下面操作
    reader.addEventListener('load', function (e) {
        const base64Image = reader.result
        callback && callback(base64Image) // 将读取到的结果传递给回调函数中
        reader = null
    })
}
```

这里我们要将上传获取的_file_进行编码转换，通过**FileReaader**[1]构造函数中的 **readAsDataURL**[2]方法进行转换，将编码后的格式文件传递给回调函数中准备进行压缩处理

### 重点的压缩图片函数

上面的代码都比较好理解，这里较为复杂的逻辑就都在**covertImageToBase64**函数的的回调**compress**中，这个功能也是压缩图片中最为核心的部分。

```js
function compress(base64Image, callback) {
    let maxW = 800 // 图片最大宽度
    let maxH = 800 // 图片最大高度
    const image = new Image()
    image.src = base64Image
    document.body.appendChild(image)
    image.addEventListener('load', function (e) {
        let ratio // 图片压缩比 
        let needCompress = false // 是否需要压缩
        // 按照比例进行图片宽高的修改
        if (image.naturalWidth > maxW ) {
            needCompress = true
            ratio = image.naturalWidth / maxW
            maxH = image.naturalHeight / ratio
         }
         if (maxH < image.naturalHeight) {
            needCompress = true
            ratio = image.naturalHeight / maxH
            maxW = image.naturalWidth / ratio
          }
         if (!needCompress) {
            maxH = image.naturalHeight
            maxW = image.naturalWidth
          }
          const canvas = document.createElement('canvas')
          canvas.setAttribute('id', '__compress__')
          canvas.width = maxW
          canvas.height = maxH
          canvas.style.visibility = 'hidden'
          document.body.appendChild(canvas)
          const ctx = canvas.getContext('2d')
          ctx.clearRect(0, 0, maxW, maxH)
          ctx.drawImage(image, 0, 0, maxW, maxH)
          const compressImage = canvas.toDataURL('image/png', 0.9)
          callback && callback(compressImage)
          // console.log(compressImage)
          canvas.remove()
        })
}
function uploadServer(compressImage) {
    console.log(`upload to server ${compressImage}`)
}
```

#### 代码分析

当我们接收到传来的base64编码的图片时，可以对其进行压缩，根据自定义的宽高与图片实际宽高对比进行比例计算，通过创建canvas元素画出该图片，并通过**toDataURL**[3] 方法将计算宽高后也就是压缩后的base64返回，传入回调中就可以进行服务器上传了，那么通过检验也是能够看到图片确实进行了压缩

