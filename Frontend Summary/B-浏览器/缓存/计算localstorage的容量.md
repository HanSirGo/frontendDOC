# 使用js代码计算localstorage的容量

- - 

要使用JavaScript代码计算`localStorage`的实际可用容量，可以采用以下步骤编写一个函数来尝试逐次增加存储项直到达到浏览器设定的限制，记录下此时已使用的总字节数，以此来估算`localStorage`的容量。

请注意，由于不同浏览器对`localStorage`容量的具体限制可能有细微差别，这种方法只能得到一个近似值。

下面是一个简单的示例代码片段，展示了如何实现这一过程：

```js
function calculateLocalStorageCapacity() {
    // 初始化变量
    let totalBytes = 0;
    const key = 'capacity_test_key'; // 使用一个临时键进行测试
    const valueSize = 10240; // 每次尝试写入的值大小，这里以10KB为例

    try {
        // 清除之前可能存在的测试数据
        localStorage.removeItem(key);

        // 开始循环写入数据
        while (true) {
            try {
                // 设置一个固定大小的值到localStorage
                const value = new Array(valueSize + 1).join(' '); // 创建一个由空格组成的字符串，长度等于valueSize
                localStorage.setItem(key, value);

                // 成功写入后，更新已用总字节数
                totalBytes += valueSize;

                // 如果浏览器支持，可以使用`size`属性直接获取当前localStorage占用的总字节数
                if ('size' in localStorage) {
                    totalBytes = localStorage.size;
                    break; // 直接使用浏览器提供的值，停止循环
                }
            } catch (error) {
                // 当超出容量限制时，catch到异常，此时totalBytes即为已使用的最大字节数
                break;
            }
        }

        // 清除测试数据
        localStorage.removeItem(key);

        return totalBytes;
    } catch (error) {
        console.error('An unexpected error occurred while calculating localStorage capacity:', error);
        return null; // 或者返回一个表示失败的值
    }
}

// 调用函数计算并打印localStorage容量
const capacity = calculateLocalStorageCapacity();
console.log('localStorage capacity:', capacity, 'bytes');
```

### 代码解析：

1.  定义一个临时键和每次写入的值大小（这里是10KB）。
2.  清除之前可能存在的测试数据。
3.  使用一个无限循环尝试逐次写入固定大小的值到`localStorage`。
4.  每次成功写入后，累加已使用的字节数。
5. 当达到浏览器的容量限制时，`setItem`操作会抛出异常，捕获此异常并退出循环，此时`totalBytes`即为已使用的最大字节数。
6.  清除用于测试的临时数据。
7.  返回计算得出的`localStorage`容量。

如果浏览器支持并提供了`localStorage.size`属性（这是一个非标准特性，部分浏览器可能支持），则直接使用该值作为实际容量，避免了循环写入的过程。

请根据实际需求调整代码中的`valueSize`值和异常处理逻辑。

注意，执行这段代码会占用`localStorage`，因此在生产环境中使用时需确保不会干扰到应用的正常数据存储。

另外，由于浏览器对`localStorage`容量的限制可能随版本更新而变化，定期重新计算或使用浏览器原生提供的接口（如果有的话）获取最新容量更为准确。

更多详细内容，请**微信搜索“前端爱好者“**， **戳我 查看** 。

## 如何使用js代码计算localstorage的当前容量

要使用JavaScript代码计算`localStorage`当前已使用的容量，你可以遍历所有存储的键值对，计算它们各自的大小，并将这些大小相加。

由于`localStorage`中存储的都是字符串，可以利用字符串的`length`属性来获取其字节长度。

考虑到实际存储时字符串可能会被序列化（如存储JSON对象时），应确保计算时考虑序列化后的字符串长度。以下是计算`localStorage`当前容量的示例代码：

```js
function calculateCurrentLocalStorageUsage() {
    let totalBytes = 0;

    for (let i = 0; i < localStorage.length; i++) {
        const key = localStorage.key(i);
        const value = localStorage.getItem(key);

        // 对于非字符串值（如JSON对象），确保计算序列化后的字符串长度
        const serializedValue = typeof value === 'string' ? value : JSON.stringify(value);
        
        totalBytes += serializedValue.length;
    }

    return totalBytes;
}

// 调用函数计算并打印localStorage当前已使用的容量
const usage = calculateCurrentLocalStorageUsage();
console.log('localStorage current usage:', usage, 'bytes');
```

### 代码解析：

1. 初始化变量`totalBytes`为0，用于累计已使用的字节数。
2. 遍历`localStorage`中的所有键，使用`localStorage.key(i)`和`localStorage.getItem(key)`获取键名和对应值。
3.  对于每个值，如果是非字符串类型（可能为JSON对象或其他可序列化的数据），使用`JSON.stringify()`将其转化为字符串，确保计算其序列化后的长度。如果已经是字符串，则直接使用该值。
4. 将当前值的长度（或序列化后的长度）累加到`totalBytes`中。
5. 遍历完成后，返回`totalBytes`作为`localStorage`当前已使用的容量。

注意，这种方法计算的是实际存储在`localStorage`中的字符串所占用的字节数，而非`localStorage`总容量。

浏览器对`localStorage`的容量限制可能因版本或环境（如私密模式）的不同而有所变化，因此实际可用容量可能会与常见的5MB有所出入。

