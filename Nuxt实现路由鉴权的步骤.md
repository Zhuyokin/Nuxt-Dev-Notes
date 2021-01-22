# Nuxt实现路由鉴权的步骤

## 需求

将Vue-Cli迁移到Nuxt的过程中,在部分页面如个人设置setting页面,假如路由跳转到此页面，对于未登录的用户需要跳转到/login登陆页面,对于已登陆的用户则正常访问,我们因此需要做路由鉴权.



## 代码思路 

1.针对需要鉴权的页面和不需要鉴权的页面设置Meta标签,其变量为loginRequired,loginRequired为true表示该页面需要登陆才能访问,为false表示不需要登陆即可访问.

```setting.vue```文件

```javascript
export default {
  meta: {
    loginRequired: true,
  },
}
```

2.中间件middleware文件夹创建routeAuth.js进行路由鉴权,一种思路是vue-router自带的路由导航守卫,需要注意的是vue-router的beforeEach不包含next方法,本文采用另一种方式

```javascript
export default ({
    route,
    redirect,
    store
}) => {
    // 如果vuex储存了用户信息,且用户信息字符串不为空对象(刷新页面)||用户信息为有user_id的js对象(页面初始化以后用户信息为js对象)
    if (store.state.userinfo && (store.state.userinfo.length > 1 || store.state.userinfo.user_id)) {
        return
    } else {
        // 未登录状态用户若遇到需要鉴权的页面则跳转到/login
        if (route.meta[0].loginRequired) {
            redirect('/login')
                // window.location.href = "https://www.careerinsights.net/login"
        } else {
            return
        }
    }
}
```

3.由于刷新页面会导致store里的userinfo数据消失故而需要使用Nuxt的nuxtServerInit()获取请求头的Cookies里面的userinfo并在初始化的时候储存在store中。store文件夹下的index.js文件action部分的代码如下：

```javascript
    actions: {
        // nuxtServerInit({ commit },{ dispatch }, { req }) {
        async nuxtServerInit({
            commit
        }, {
            req,
            app,
            store
        }) {
            var arrCookie = unescape(req.headers.cookie).split("; ");
            for (var i = 0; i < arrCookie.length; i++) {
                var arr = arrCookie[i].split("=");
                if (arr[0] == 'userinfo') {
                    commit('SET_USERINFO', arr[1]);  // 储存请求头携带的Cookies里的UserInfo
                }
            }
            var config = await getConfig({
                lang: store.state.locale
            });
            commit('SET_CONFIG', config.data);
            commit('SET_CONFIGVISIBLE', true);
            commit('SET_LANG', "zh");
        },
    },
```

## 参考/扩展资料

```html
Nuxt.js 導航守衛
https://hsiangfeng.github.io/vue/20190810/3385236004/
https://www.dazhuanlan.com/2019/09/30/5d91b5441bced/
```

