## wangEditor富文本编辑器

```js

yarn add @wangeditor/editor
# 或者 npm install @wangeditor/editor --save

vue2组件安装
yarn add @wangeditor/editor-for-vue
# 或者 npm install @wangeditor/editor-for-vue --save
```

```vue

<template>
    <div style="border: 1px solid #ccc;">
        <Toolbar
            style="border-bottom: 1px solid #ccc"
            :editor="editor"
            :defaultConfig="toolbarConfig"
            :mode="mode"
        />
        <Editor
            style="height: 500px; overflow-y: hidden;"
            v-model="html"
            :defaultConfig="editorConfig"
            :mode="mode"
            @onCreated="onCreated"
        />
    </div>
</template>
<script>
import Vue from 'vue'
import { Editor, Toolbar } from '@wangeditor/editor-for-vue'

export default Vue.extend({
    components: { Editor, Toolbar },
    data() {
        return {
            editor: null,
            html: '<p>hello</p>',
            toolbarConfig: { //工具栏的配置
              excludeKeys: ["group-video"],//排除某些菜单，其他都保留
            },
            editorConfig: { //修改菜单配置
              placeholder: '请输入内容...' 
              MENU_CONF：{
                uploadImage：{//设置图片上传配置
                    server: '/api/upload-image',上传路径
               fieldName: 'custom-field-name'上传文件名
               // 单个文件的最大体积限制，默认为 2M
                        maxFileSize: 1 * 1024 * 1024, // 1M

                        // 最多可上传几个文件，默认为 100
                        maxNumberOfFiles: 10,

                        // 选择文件时的类型限制，默认为 ['image/*'] 。如不想限制，则设置为 []
                        allowedFileTypes: ['image/*'],

                        // 自定义上传参数，例如传递验证的 token 等。参数会被添加到 formData 中，一起上传到服务端。
                        meta: {
                            token: 'xxx',
                            otherKey: 'yyy'
                        },

                        // 将 meta 拼接到 url 参数中，默认 false
                        metaWithUrl: false,

                        // 自定义增加 http  header
                        headers: {
                            Accept: 'text/x-json',
                            otherKey: 'xxx'
                        },

                        // 跨域是否传递 cookie ，默认为 false
                        withCredentials: true,

                        // 超时时间，默认为 10 秒
                        timeout: 5 * 1000, // 5 秒
                }
              }
            },
            mode: 'default', // or 'simple'
        }
    },
    methods: {
        onCreated(editor) {
          let vm = this;
            this.editor = Object.seal(editor) // 一定要用 Object.seal() ，否则会报错
            
            
      // 插入图片
      this.editor.getMenuConfig("uploadImage").customInsert = function (
            res,
            insertFn
) {
              //可以对返回的res进行处理之后传入给insertFn()
              insertFn(res.data.url);
          };
        },
        
        //上传失败后的回调函数
      this.editor.getMenuConfig("uploadImage").onError = function (
        file,
        err,
        res
) {
        if (file.size > vm.editorConfig.MENU_CONF.uploadImage.maxFileSize) {
          vm.$message.error("图片大小不能大于2M");
        }else{
           vm.$message.error(err);
        }
      };
    },
    mounted() {
        // 模拟 ajax 请求，异步渲染编辑器
        setTimeout(() => {
            this.html = '<p>模拟 Ajax 异步设置内容 HTML</p>'
        }, 1500)
    },
    beforeDestroy() {
        const editor = this.editor
        if (editor == null) return
        editor.destroy() // 组件销毁时，及时销毁编辑器
    }
})
</script>
```

