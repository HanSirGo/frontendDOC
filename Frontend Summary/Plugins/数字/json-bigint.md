## json-bigint

```js
# 解决js精度丢失问题,js中的Number类型只能安全的表示 -（2^53 - 1) 和 2^53-1 之间的整数,任何超出此范围的整数值都可能失去精度.

npm i json-bigint

import axios from 'axios'
import JSONbig from 'json-bigint'
const JSONbigToString = JSONbig({storeAsString:true})

const _axios = axios.create({
    timeout:6*60*1000,
    transformResponse: [
        function(data) {
            try{
                // 转换
                return JSONbigToString.parse(data)
            } catch(err) {
                // 转换失败就直接返回元数据
                return data
            }
        }
    ]
})
```

