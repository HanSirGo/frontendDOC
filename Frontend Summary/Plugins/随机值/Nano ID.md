## Nano ID

nanoid 是一个小巧、安全、URL友好、唯一的 JavaScript 字符串ID生成器。其具有以下特性：

- **小巧.** 130 bytes (已压缩和 gzipped)。没有依赖。Size Limit 控制大小。
- **快速.** 它比 UUID 快 60%。
- **安全.** 它使用加密的强随机 API。可在集群中使用。
- **紧凑.** 它使用比 UUID（A-Za-z0-9_-）更大的字母表。因此，ID 大小从36个符号减少到21个符号。
- **易用.** Nano ID 已被移植到 20种编程语言。

```js
import { nanoid } from 'nanoid'
model.id = nanoid() //=> "V1StGXR8_Z5jdHi6B-myT"
```

**Github：**https://github.com/ai/nanoid

