# Nuxt引入vue-quill-editor富文本编辑器



## 需求

将vue-cli迁移到Nuxt的过程中,原富文本插件vue-html5-editor各种报错，这是由于其不存在SSR版本造成的，故而需要更换富文本插件,经过多方对比我们采用了vue-quill-editor(支持SSR)，富文本内容需要过滤掉图片和样式(正则),Nuxt引入vue-quill-editor的步骤如下

## 代码

在plugins文件夹下新建文件vue-quill-editor.js

```javascript
import Vue from 'vue'
import VueQuillEditor from 'vue-quill-editor/dist/ssr'

Vue.use(VueQuillEditor)
```

在nuxt.config.js配置vue-quill-editor

```javascript
 css: ['element-ui/lib/theme-chalk/index.css',
        'quill/dist/quill.snow.css',
        'quill/dist/quill.bubble.css',
        'quill/dist/quill.core.css'
    ],

plugins: [{
            src: '@/plugins/element-ui',
            ssr: true
        }, {
            src: '@/plugins/i18n.js',
            ssr: true
        }, {
            src: '@/plugins/prototype.js',
            ssr: false
        },
        {
            src: "@/plugins/vue-quill-editor",
            ssr: false
        },
    ],
```

在page目录的vue文件中使用

```javascript
		<div
          style="height: 300px; overflow-y: auto;"
          class="quill-editor"
          :content="resumeForm.summary"
          @change="updateDataSummary"
          v-quill:myQuillEditor="editorOption"
        ></div>


data() {
    return {
      resumeForm: {
        summary: "",
      },
      show: true,
      editorOption: {
        // some quill options
        modules: {
          toolbar: [
            ["bold", "italic", { list: "ordered" }, { list: "bullet" }],
          ],
        },
      },
    };
  },
      
      
updateDataSummary({ editor, html, text }) {
      // console.log(editor, html, text);
      // 过滤style,class和图片
      //  content.innerHTML = content.innerHTML.replace(/style\s*=(['\"\s]?)[^'\"]*?\1/gi,"").replace(/class\s*=(['\"\s]?)[^'\"]*?\1/gi,"").replace(/\s*=(['\"\s]?)[^'\"]*?\1/gi,"");
      this.resumeForm.summary = this.content = html.replace(/style\s*=(['\"\s]?)[^'\"]*?\1/gi,"").replace(/\s*=(['\"\s]?)[^'\"]*?\1/gi,"");;
    },
```

## 参考/扩展资料

```html
nuxt.js中使用vue-quill-editor富文本编辑器
https://blog.csdn.net/weixin_36185028/article/details/82946453
https://www.npmjs.com/package/vue-quill-editor
https://www.shangmayuan.com/a/9bc767d896f84130a9188fef.html
https://blog.csdn.net/weixin_30699955/article/details/95204918
https://www.cnblogs.com/chrislinlin/p/12587723.html
vue-quill-editor富文本编辑器使用
https://www.jianshu.com/p/a6cba69d6e49
https://blog.csdn.net/senmage/article/details/82388728
```

