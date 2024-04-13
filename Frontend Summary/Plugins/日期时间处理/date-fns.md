## date-fns

date-fns 是一个现代的 JavaScript 日期工具类库，提供了最全面、最简单和一致的工具集，用于在浏览器和 Node.js 中操作 JavaScript 日期。其具有以下特性：

- **模块化**：根据需求选择需要引用的模块
- **不可变**：date-fns 使用纯函数构建，并且始终返回一个新的日期实例，而不是更改传递的日期实例。它允许防止错误并跳过长时间的调试会话
- **可信赖**：遵循语义版本，始终向后兼容
- **快速**：轻量快速，为用户提供最佳的使用体验
- **TypeScript & Flow**：date-fns 同时支持 Flow 和 TypeScript

```js
import { format, formatDistance, formatRelative, subDays } from 'date-fns'

format(new Date(), "'Today is a' eeee")
//=> "Today is a Saturday"

formatDistance(subDays(new Date(), 3), new Date(), { addSuffix: true })
//=> "3 days ago"

formatRelative(subDays(new Date(), 3), new Date())
//=> "last Friday at 7:26 p.m."
```

**Github：**https://github.com/date-fns/date-fns

