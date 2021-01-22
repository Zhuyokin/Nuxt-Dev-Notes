# Nuxt的asyncData方法的参数

## 需求

Nuxt的asyncData方法的参数有哪些？

## 详解

```javascript
  async asyncData({app,store,route,params,query,store,error,env,isDev,isHMR}){
    // console.log(params, query, error, store,app);
    console.log(query.jobid);
    // console.log(context.store.state);
    // console.log(store);
    let [job_detail, joblist] = await Promise.all([
      getJobInfoByJobID({job_id: query.jobid}),
      searchJobList({})
    ]);
    return {
      job_detail: job_detail.data,
      joblist: joblist.data,
    };
  },
  // app代表当前页面组件对象;store:vuex对象;route,params,query:页面路由参数;error:错误对象
 // 这些参数都是可选的
```

## 参考/扩展资料

```html
nuxtjs asyncData参数
https://blog.csdn.net/cow66/article/details/103908614
```

