# Nuxt.js中关闭eslint语法检测

## 需求

在nuxt项目中,开启了eslint语法检测会频繁报错,如何关闭eslint语法检测呢?

## 代码

在Nuxt最新版本2.14.0,注释掉nuxt.config.js的eslint插件即可

```html
buildModules: [
        // Doc: https://github.com/nuxt-community/eslint-module
        // '@nuxtjs/eslint-module',
        '@nuxtjs/router',
    ],
```

## 参考扩展资料

```html
关于Nuxt.js中关闭eslint语法检测
https://www.jianshu.com/p/2a0b829a5a05
```

