# 如何获取 blob里面的值

## 1. 如何获取 blob里面的值

要从`Blob`对象中获取其内容，你需要根据`Blob`的内容类型采取不同的策略。

下面是一些常见的情况和对应的处理方法：

### 1.1. JSON 数据

如果`Blob`包含的是JSON格式的数据，你可以使用`TextDecoder`来读取它，并将其解析为JSON。

```
const blob = new Blob([/* ... */], { type: 'application/json' });

// 创建一个读取器
const reader = new FileReader();

reader.onloadend = function() {
  const jsonContent = this.result; // 这里是字符串形式的JSON
  try {
    const jsonData = JSON.parse(jsonContent);
    console.log(jsonData); // 解析后的JSON对象
  } catch (error) {
    console.error('Error parsing JSON', error);
  }
};

reader.readAsText(blob); // 读取Blob作为文本
```

### 1.2. 文本数据

如果`Blob`包含的是普通的文本数据，同样可以使用`FileReader`的`readAsText`方法。

```
const blob = new Blob([/* ... */], { type: 'text/plain' });

const reader = new FileReader();

reader.onloadend = function() {
  const textContent = this.result;
  console.log(textContent); // 文本内容
};

reader.readAsText(blob); // 读取Blob作为文本
```

### 1.3. 图像数据

如果`Blob`包含的是图像数据，你可以使用`URL.createObjectURL`来创建一个临时的URL，然后在`<img>`标签中显示这个URL。

```
const blob = new Blob([/* ... */], { type: 'image/jpeg' });

const imageUrl = URL.createObjectURL(blob);

const imgElement = document.createElement('img');
imgElement.src = imageUrl;
document.body.appendChild(imgElement); // 显示图像
```

### 1.4. 其他二进制数据

对于其他类型的二进制数据，可以使用`readAsArrayBuffer`方法来读取。

```
const blob = new Blob([/* ... */], { type: 'application/octet-stream' });

const reader = new FileReader();

reader.onloadend = function() {
  const arrayBuffer = this.result;
  // 在这里可以使用arrayBuffer进行进一步处理
  // 比如转换为Uint8Array或其他形式
};

reader.readAsArrayBuffer(blob);
```

### 1.5. 示例

假设你有一个`fetch`请求返回了一个`Blob`对象，你可以这样处理：

```
fetch('/your-endpoint')
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.blob(); // 获取Blob对象
  })
  .then(blob => {
    // 根据Content-Type确定如何处理
    if (blob.type.startsWith('application/json')) {
      // 处理JSON数据
      const reader = new FileReader();
      reader.onloadend = () => {
        const jsonContent = reader.result;
        const jsonData = JSON.parse(jsonContent);
        console.log(jsonData);
      };
      reader.readAsText(blob);
    } else if (blob.type.startsWith('image/')) {
      // 处理图像数据
      const imageUrl = URL.createObjectURL(blob);
      const imgElement = document.createElement('img');
      imgElement.src = imageUrl;
      document.body.appendChild(imgElement);
    } else if (blob.type === 'text/plain') {
      // 处理纯文本数据
      const reader = new FileReader();
      reader.onloadend = () => {
        const textContent = reader.result;
        console.log(textContent);
      };
      reader.readAsText(blob);
    } else {
      // 处理其他类型的数据
      const reader = new FileReader();
      reader.onloadend = () => {
        const arrayBuffer = reader.result;
        console.log(new Uint8Array(arrayBuffer)); // 以Uint8Array的形式显示数据
      };
      reader.readAsArrayBuffer(blob);
    }
  })
  .catch(error => console.error('There has been a problem with your fetch operation:', error))
```

请注意，在处理完`Blob`之后，记得释放创建的临时URL以避免内存泄漏：

```
// 当不再需要时URL.revokeObjectURL(imageUrl);
```

