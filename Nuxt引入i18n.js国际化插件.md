# Nuxt引入i18n.js国际化插件

## 需求

将vue-cli迁移到nuxt的过程中,需要对原本的i18n.js国际化插件的配置进行修改,Nuxt引入国际化插件的步骤如下



## 步骤

1.在plugins文件夹下新建i18n.js文件

```javascript
import Vue from 'vue'
import VueI18n from 'vue-i18n'
// import store from '@/store/index.js'

Vue.use(VueI18n)

export default ({ app, store }) => {
    // Set i18n instance on app
    // This way we can use it in middleware and pages asyncData/fetch
    app.i18n = new VueI18n({
        locale: store.state.locale,
        fallbackLocale: 'en', // 我这里默认语言为英文
        messages: {
            'en': require('@/locales/en.json'),
            'zh': require('@/locales/zh.json')
        }
    })

}

```

2.在根目录locale文件夹下新建国际化中英文json格式文件

```json
{
  "links": {
    "home": "Home",
    ...
  },
  "home": {
    "title": "Welcome",
    "introduction": "This is an introduction in English."
  },
  "about": {
    "title": "About",
    "introduction": "..."
  }
}
```



3.在nuxt.config.js配置i18n.js

```javascript
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

## 参考/扩展资料

```html
nuxt.js实战之用vue-i18n实现多语言
https://www.cnblogs.com/chunshan-blog/p/9955111.html
nuxtjs 国际化i18n nuxt国际化（vue-i18n + element-ui）初步踩坑
https://blog.csdn.net/Py1807A/article/details/102416954?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.add_param_isCf&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.add_param_isCf
https://blog.csdn.net/weixin_30576859/article/details/95707137?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.add_param_isCf&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.add_param_isCfhttps://www.freesion.com/article/7196148362/
https://blog.csdn.net/qq_41614928/article/details/108677288
http://www.tianshan277.com/700.html?replytocom=1009
https://www.cnblogs.com/chunshan-blog/p/9955111.html
https://www.jianshu.com/p/c86f5a383ecc
http://www.manongjc.com/detail/15-bxhangktrnyrvco.html
https://www.it610.com/article/1296910080616243200.htm
https://www.cnblogs.com/bien94/p/12320827.html
```

