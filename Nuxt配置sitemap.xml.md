# Nuxt配置sitemap.xml

## 背景

Nuxt上线后考虑到SEO需要加上sitemap.xml.

## 详解

```javascript
// 安装插件@nuxtjs/sitemap
npm i @nuxtjs/sitemap -D
cnpm i @nuxtjs/sitemap -D
// 在项目根目录下找到 nuxt.config.js 往modules添加'@nuxtjs/sitemap',并
    modules: [
        // Doc: https://axios.nuxtjs.org/usage
        '@nuxtjs/axios',
        // '@nuxtjs/router',
        '@nuxtjs/sitemap',
        '@nuxtjs/robots',
    ],
// 配置相关的设置项,@nuxtjs/sitemap 网站地图
    sitemap: {
        gzip: true,
        defaults: {
            changefreq: 'daily',
            priority: 1,
            lastmod: new Date()
        }
    },
```

## 参考/扩展资料

```html
Nuxt 配置sitemap.xml
https://blog.csdn.net/qq_40776187/article/details/109314369
https://www.cnblogs.com/kongyijilafumi/p/14150731.html
```

