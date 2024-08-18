# 如何用JavaScript检测图片的宽高

![1723975013930](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723975013930.png)

大家好，今天我们来聊聊一个非常实用的小技巧——如何用JavaScript检测用户上传的图片的宽度和高度。这在我们开发中非常常见，比如说，我们要限制用户上传头像的尺寸，或者是对商品图片进行尺寸检查。这些场景都要求我们在前端就能搞定图片尺寸的检测，避免用户上传不符合要求的图片。

# **业务需求：用户头像上传**

想象一下，你正在开发一个用户头像上传的功能。我们希望用户上传的头像不能超过100x100像素，这样可以确保头像在页面上显示得清晰又美观。那我们该怎么实现呢？

# **解决方案：FileReader和Image对象**

不用担心，JavaScript提供了非常方便的FileReader和Image对象，可以帮助我们轻松实现这个功能。具体步骤如下：

1. **获取文件输入框**：首先，我们需要获取到用户选择的文件。
2. **读取文件内容**：然后，通过FileReader对象读取文件内容。
3. **创建图片对象**：接下来，用Image对象加载读取到的文件。
4. **检测图片尺寸**：最后，通过Image对象获取图片的宽度和高度，并进行判断。

# **实现代码**

我们来看看具体的实现代码：

```
const fileUpload = document.querySelector('input[type="file"]');
const reader = new FileReader();

fileUpload.addEventListener('change', () => {
  reader.readAsDataURL(fileUpload.files[0]);
  reader.onload = (e) => {
    const image = new Image();
    image.src = e.target.result;
    image.onload = () => {
      const { height, width } = image;
      if (height > 100 || width > 100) {
        alert("上传的图片尺寸过大，请选择100x100像素以内的图片。");
      } else {
        alert("图片上传成功！");
      }
    };
  };
});
```

## **代码解析**

1. **获取文件输入框**：`document.querySelector('input[type="file"]')`选择了页面上的文件输入框。
2. **创建FileReader实例**：通过`new FileReader()`创建了一个FileReader对象。
3. **添加事件监听器**：`fileUpload.addEventListener('change', () => { ... })`监听文件输入框的变化事件，当用户选择文件时触发。
4. **读取文件内容**：`reader.readAsDataURL(fileUpload.files[0])`读取用户选择的文件，并转换为Data URL格式。
5. **创建Image对象**：在FileReader读取完成后，通过`new Image()`创建一个图片对象，并将其`src`属性设置为读取到的文件内容。
6. **检测图片尺寸**：当图片加载完成后，通过`image.onload`事件获取图片的宽度和高度，并进行尺寸判断。

# **结束**

通过以上步骤，我们可以轻松实现对用户上传图片尺寸的检测。这不仅提高了用户体验，还能确保上传的图片符合我们的要求。希望这个小技巧能对你有所帮助，赶紧在你的项目中试试吧！