# markdown 编辑器 vditor 在 vue3 中的基本使用和扩展

> ❝
>
> 官网：https://b3log.org/vditor/

## **使用背景**

前段时间开发功能，需要内嵌 markdown 编辑器以编辑内容，便调研了 markdown 编辑器相关插件，最终选定 vditor。

在实际使用过程中，遇到了一些问题，同时积累了一些解决经验，在此作以分享。

## **功能简介**

Vditor 是一款浏览器端的 Markdown 编辑器，支持所见即所得、即时渲染（类似 Typora）和分屏预览模式。它使用 TypeScript 实现，支持原生 JavaScript、Vue、React、Angular，提供桌面版。提供以下功能，基本满足需求：

- 支持三种编辑模式：所见即所得（wysiwyg）、即时渲染（ir）、分屏预览（sv）
- 支持大纲、数学公式、脑图、图表、流程图、甘特图、时序图、五线谱、多媒体、语音阅读、标题锚点、代码高亮及复制、graphviz、PlantUML 渲染
- 实现 CommonMark 和 GFM 规范，可对 Markdown 进行格式化和语法树查看，并支持10+项配置
- 工具栏包含 36+ 项操作，除支持扩展外还可对每一项中的快捷键、提示、提示位置、图标、点击事件、类名、子工具栏进行自定义
- 多语言支持，内置中、英、韩文本地化
- 支持主流浏览器，对移动端友好
- ……

## **基本使用**

### **安装**

```
pnpm install --save-dev vditor
```

### **使用**

```
<template>
  <!-- 指定一个容器 -->
  <div id="vditor"></div>
</template>
<script lang="ts" setup>
// 1.1 引入Vditor 构造函数
import Vditor from 'vditor'
// 1.2 引入样式
import 'vditor/dist/index.css'
import { ref, onMounted } from 'vue'

// 2. 获取DOM引用
const vditor = ref<Vditor | null>(null)

// 3. 在组件初始化时，创建Vditor对象，并引用
// new Vditor() 中传入两个参数，一个是容器id，一个是配置option
onMounted(() => {
  vditor.value = new Vditor('vditor', {
    height: '50vh',
    width: '50vw',
  })
})
</script>
```

### **常用配置项**

以下列举常用的配置项，详细可见官网：https://ld246.com/article/1549638745630#API

1. height、width: 定义编辑器的尺寸
2. toolbar：工具栏，选取需要的按钮或自定义按钮
3. outline: 大纲，定义大纲的显示与否、左右位置
4. placeholder: 编辑器内容为空时的默认显示
5. mode: 默认的编辑模式，所见即所得（wysiwyg）、即时渲染（ir）、分屏预览（sv）

### **常用 API**

以下列举常用的 API，详细可见官网：https://ld246.com/article/1549638745630#API

1. getValue()：获取编辑器的 markdown 内容
2. setValue()：设置编辑器的 markdown 内容
3. disabled()：禁用编辑器
4. enabled()：编辑器解除禁用
5. after()：编辑器异步渲染完成后的回调方法

## **功能扩展**

### **离线环境集成**

由于 vditor 采用按需加载机制，默认 CDN 为cdn.jsdelivr.net/npm/vditor@版本号，在内网环境下，或者在线 CDN 挂掉的情况下，编辑器无法正常显示。这时候有两种解决方案：自建 CDN 或打包到项目中

#### **自建 CDN**

参考：https://juejin.cn/post/7100769542745358367

#### **打包到项目中**

1. 复制 dist：将node_modules/vditor/dist复制到需要部署的服务的文件夹中，比如，我这里用的是 vite 所以就是 public 文件夹下，理论上和 index.html 是同级的。

2. 优化：

3. 1. 加载 js 很慢，进行压缩
   2. 打包后文件体积较大，删除 dist 中不需要的组件源码

### **自定义文件上传与校验**

由于 vditor 编辑器自身的上传文件校验提示的 UI 风格与需求不适配，所以在此需要自定义文件上传与校验。

```
vditor.value = new Vditor('vditor', {
  upload: {
    url: `${MANUAL_FILE_PEER}/upload?uploadDirectoryPath=/manual/${props.manualCode}`, // 文件上传接口
    accept: '.jpg,.png', // 限制可上传的文件类型
    max: 10 * 1024 * 1024, // 限制文件大小
    multiple: false,
    fieldName: 'file',
    // 文件名安全处理
    filename(name) {
      return (
        name
          // eslint-disable-next-line no-useless-escape
          .replace(/[^(a-zA-Z0-9\u4E00-\u9FA5\.)]/g, '')
          // eslint-disable-next-line no-useless-escape
          .replace(/[\?\\/:|<>\*\[\]\(\)\$%\{\}@~]/g, '')
          .replace('/\\s/g', '')
      )
    },
    // 对文件进行二次处理，此处主要是重命名文件，避免文件名重复
    file(files: File[]) {
      const file = files[0]
      const fileName = nanoid() + getFileType(file.name)
      return [new File([file], fileName, { type: file.type })]
    },
    // 数据转换
    format(files, responseText) {
      const res = JSON.parse(responseText)
      const url = `${MANUAL_FILE_UPLOAD_PEER}/manual/${props.manualCode}/${res.filename}`
      const result = JSON.stringify({
        code: 0,
        data: {
          errFiles: [],
          succMap: { [res.filename]: url },
        },
      })
      return result
    },
    // 自定义文件校验，返回true或false，并提示相关信息
    validate(files: File[]) {
      const file = files[0]
      return customFileValidate(file)
    },
  },
  after: () => {
    // 自定义文件上传相关样式
    customFileStyle()
  },
})
```

customFileStyle()方法主要用于自定义文件上传相关样式，源码为以下：

```
// 校验文件类型 accept
const setFileValidateType = () => {
  const fileInput = document.querySelector('input[type="file"]')
  fileInput?.setAttribute('accept', '.jpg,.png')
}

// 去掉input file文件上传提示
const deleteFileInputTitle = () => {
  const fileInput: HTMLElement = document.querySelector(
    'input[type="file"]'
  ) as HTMLElement
  if (fileInput) {
    fileInput.addEventListener('mouseover', () => {
      fileInput.title = ''
    })
  }
}

// 自定义文件上传样式
export const customFileStyle = () => {
  setFileValidateType()
  deleteFileInputTitle()
}
```

其中，setFileValidateType()主要用于设置选择文件时，文件资源管理器的类型限制。由于使用文件自定义校验，accept 失效，所以要加上该属性用于类型限制。deleteFileInputTitle()主要用于去掉浏览器自带的文件上传提示。

### **全屏适配**

由于 vditor 编辑器所嵌入的页面属于应用工具，以 iframe 的方式嵌入到平台中。如果使用 vditor 自带的全屏功能，点击全屏按钮，只能将其放大到铺满 iframe 界面，而无法铺满系统界面，所以此处需要自定义全屏功能。

自定义全屏功能，最重要的是需要同步浏览器和编辑器的全屏状态。

以下是源码：

requestFullScreen()方法用于浏览器进入全屏状态；

exitFullScreen()方法用于浏览器退出全屏状态；

handleFullscreen()方法用于同步浏览器和编辑器的全屏状态。首先，通过监听fullscreenchange事件从而捕获浏览器的全屏状态变化，当编辑器状态不统一时，模拟用户点击编辑器提供的全屏按钮，从而同步全屏状态；其次，通过监听编辑器全屏按钮的点击事件从而捕获编辑器的全屏状态变化，当浏览器的状态不统一时，同步浏览器的全屏状态。

judgeVditorFullScreen()方法用于通过判断编辑器全屏按钮的图标来确认其是否处于全屏状态。

```
// 进入全屏状态
const requestFullScreen = (element: Element) => {
  if (element.requestFullscreen) {
    element.requestFullscreen()
  }
}

// 退出全屏状态
const exitFullScreen = () => {
  if (document.exitFullscreen) {
    document.exitFullscreen()
  }
}

// 全屏处理
export const handleFullscreen = () => {
  document.addEventListener('fullscreenchange', () => {
    // 非点击退出全屏时（esc或×） 需要同步更新编辑的全屏状态
    if (!document.fullscreenElement) {
      if (!judgeVditorFullScreen()) {
        const clickBtnElement = document.querySelector(
          '[data-type="fullscreen"]'
        ) as HTMLElement
        clickBtnElement?.click()
      }
    }
  })
  document
    .querySelector('[data-type="fullscreen"]')
    ?.addEventListener('click', () => {
      if (!judgeVditorFullScreen()) {
        requestFullScreen(document.documentElement)
      } else {
        exitFullScreen()
      }
    })
}

// 判断编辑器当前处于全屏/非全屏状态
const judgeVditorFullScreen = () => {
  const fullScreenBtn = document
    .querySelector('[data-type="fullscreen"]')
    ?.querySelector('svg')
    ?.querySelector('use')
  if (fullScreenBtn?.getAttribute('xlink:href') === '#vditor-icon-fullscreen') {
    return true
  } else if (
    fullScreenBtn?.getAttribute('xlink:href') === '#vditor-icon-contract'
  ) {
    return false
  }
}
```

### **撤销功能分析**

**问题现象**：快速并连续输入多行内容，点击撤销按钮，出现一次撤销多行输入的情况

**问题原因**：通过分析 vditor 源码，梳理了其撤销功能的处理机制，在此作以解释：

1. 撤销功能（Undo）和重做功能（Redo）两者互为依赖。撤销功能借助核心类 Undo 实现，该类使用 undoStack 和 redoStack 来分别存储可以撤销的操作和已经撤销的操作，提供 undo 和 redo 两个方法来执行撤销和重做，使用 DiffMatchPatch 库来计算文本差异（diff），并创建补丁（patch）用来记录编辑操作。
2. undo 方法首先检查编辑区域是否可编辑，然后从 undoStack 中弹出最近的状态，并将其推入 redoStack 中。然后使用 renderDiff 方法应用补丁到编辑器内容，从而撤销更改。
3. redo 方法正好相反。首先检查编辑区域是否可编辑，然后从 redoStack 中弹出最近的状态，并将其推回 undoStack 中，同样使用 renderDiff 方法将补丁反向应用，从而重做更改。
4. 编辑器通过 addToUndoStack 方法记录当前状态。该方法在每次编辑操作后被调用，以记录当前状态到 undoStack。如果当前文本与上次记录的文本有差异，就创建一个新的补丁并存储。如果 undoStack 的长度超过了设定的栈大小 stackSize，则移除最老的状态。

通过分析以上处理机制，我们发现，一次撤销多行输入，是因为编辑器将“多行输入”看作“一次输入”。也就是说，在用户快速连续输入多行内容时，只调用了一次 addToUndoStack()方法进行状态记录。于是，我们继续分析 addToUndoStack()的调用时机：

```
export const processAfterRender = (
  vditor: IVditor,
  options = {
    enableAddUndoStack: true,
    enableHint: false,
    enableInput: true,
  }
) => {
  // ……
  clearTimeout(vditor.sv.processTimeoutId)
  vditor.sv.processTimeoutId = window.setTimeout(() => {
    if (options.enableAddUndoStack && !vditor.sv.composingLock) {
      vditor.undo.addToUndoStack(vditor)
    }
  }, vditor.options.undoDelay)
}
```

以上代码说明，每隔vditor.options.undoDelay时间记录一次输入状态，vditor 默认undoDelay为 800ms，即用户 800ms 内的输入内容默认视为一次输入。

至此，该问题的出现原因已经分析清楚。

**解决方案**：

这个问题需要解决吗，如果需要，如何解决？

编辑器提供了 vditor 关于历史记录间隔undoDelay的配置功能，允许开发人员进行修改。但综合性能、无法完全避免该问题、调研语雀编辑器等竞品后，可以不修改该问题。