# Nuxt处理异步数据(分页)的SEO

## 背景

在展示职位或公司列表的页面,分页点击下一页后,查看源码只能展示第一页的内容(DOM元素),如何才能分页后查看源码展示对应页码的内容呢?

## 详解

在Nuxt的官方文档中,提供了一个API属性watchQuery属性,其作为一个侦听路由参数的属性,若其值发生变化,则其所在的组件内部的Nuxt方法包括(asyncData，fetch(context), validate, layout, ...)都会重新调用，为了提高性能，默认情况下禁用。如果您要为所有参数字符串设置监听， 请设置： watchQuery: true.

```javascript
export default {
  watchQuery: ['page']
}

export default {
  watchQuery(newQuery, oldQuery) {
    // Only execute component methods if the old query string contained `bar`
    // and the new query string contains `foo`
    return newQuery.foo && oldQuery.bar
  }
}
```

## 参考/扩展资料

```html
Nuxt处理异步数据(分页)的SEO
https://www.cnblogs.com/Allen-0827/p/13677748.html
https://www.w3cschool.cn/nuxtjs/nuxtjs-og5336gv.html
https://zh.nuxtjs.org/docs/2.x/components-glossary/pages-watchquery
```

