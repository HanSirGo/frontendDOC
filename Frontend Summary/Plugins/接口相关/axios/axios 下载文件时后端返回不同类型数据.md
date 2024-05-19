# axios 下载文件时后端返回不同类型数据

### 问题描述

在使用 axios 下载文件时，后端返回值可能会出现两种情况：

- 文件流：后端直接返回文件的二进制流。
- JSON 对象：后端返回一个包含文件信息或其他数据的 JSON 对象。

### 解决方案

#### 情况一：设置 arraybuffer

在请求设置为 responseType: 'arraybuffer' 的情况下，我们可以处理后端返回文件流的情况。具体步骤如下：

- • 请求成功时，直接导出文件。
- • 请求失败时，将返回的 arraybuffer 类型数据转回 JSON 对象，然后进行判断。

```js
async downloadFile(file) {
  try {
    const res = await axios.post(url, { responseType: "arraybuffer" });
    let data = res.data;
    if (!(data instanceof Blob)) {
      const enc = new TextDecoder('utf-8');
      data = JSON.parse(enc.decode(new Uint8Array(data)));
      // 如果报错进入err说明返回是文件流进入下载   不报错说明返回json
    }
    // 提示信息
  } catch (err) {
      // 下载逻辑
      const url = window.URL.createObjectURL(new Blob([res.data]));
      const link = document.createElement("a");
      link.href = url;
      link.setAttribute("download", "name");
      document.body.appendChild(link);
      link.click();
      setTimeout(() => {
        link.remove();
      }, 0);
  }
}
```

#### 情况二：设置 blob

在请求设置为 responseType: 'blob' 的情况下，我们可以处理后端返回 JSON 对象的情况。具体步骤如下：

- 将已转为 blob 类型的数据转回 JSON 对象。
- 根据 JSON 对象中的状态信息进行相应的处理。

```js
async downloadFile(file) {
  try {

     const res = axios.post(url, { responseType: "blob" });
     const reader = new FileReader();
     reader.onload = () => {
      const data = JSON.parse(reader.result);
        // 如果报错进入err说明返回是文件流进入下载   不报错说明返回json
     };
    reader.readAsText(res.data);
     // 处理逻辑
    };
    reader.readAsText(res.data);
  } catch (err) {
  // 下载逻辑
      const url = window.URL.createObjectURL(new Blob([res.data]));
      const link = document.createElement("a");
      link.href = url;
      link.setAttribute("download", "name");
      document.body.appendChild(link);
      link.click();
      setTimeout(() => {
        link.remove();
      }, 0);
  }
}
```

```js
const res = axios.post(url, { responseType: "blob" });

function downloadfile(blob,name){
    const url = window.URL.createObjectURL(new Blob(blob));
    const link = document.createElement("a");
    link.href = url;
    link.download = decodeURI(name);
    link.style.display= 'none';
    document.body.appendChild(link);
    link.click();
    link.parentNode?.removeChild(link);
    window.URL.revokeObjectURL(url);
}
```

------

通过以上两种情况的优化解决方案，我们可以在使用axios下载文件时更加简洁和高效地处理后端返回不同类型数据的情况。希望本文对您有所帮助！