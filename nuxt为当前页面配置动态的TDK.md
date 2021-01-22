# nuxt为当前页面配置动态的TDK

背景

当前的需求是针对职位详情,公司详情等页面设置动态的Title，Description和Keywords

```html
SEO优化:

职位页面:

职位页面title：XXX(company) is recruiting XXXX(job title) in XXX(city)- CareerInsights
职位页面keywords：XXX(job title), XXX(company), looking for XXX jobs in XXX(city)
职位页面description：XXX is recruiting XXX in XXX. the salary range is between XX and XX, the benefits contained XXX, XXX. Find more XXX opportunities in China on CareerInsights.net

公司页面:

公司页面title：XXX(company) is recruiting on CareerInsights
公司页面keywords：XXX(company), jobs in XXX(company), Find more China company review in CareerInsights.net
公司页面description：(公司简介)

首页:

主页页面title：CareerInsights | We connect international professionals with opportunities in China. 致力于为中国企业引进专业领域优秀外籍人才
主页页面keywords：外籍人才招聘, 招聘外国人, jobs in China, China expats
主页页面Description: 致力于为中国企业引进专业领域优秀外籍人才 We connect international professionals with opportunities in China. 
```

代码

由于在入口文件中已经设置了通用的TDK,针对这些动态的TDK,我们可以在单独的vue文件下的head()进行相应的设置,对于TDK里面的动态数据必须由asyncData方法获得才能正常显示出来。

公司详情

```javascript
  // SEO动态TDK参数
  head(){
    return {
      title:  this.seo_title,
      meta:[
        {
          // hid: 'keywords',
          name:"keywords",
          content:this.seo_keywords
        },
        {
          // hid: 'description',
          name:"description",
          content:this.seo_description
        }
      ]
    }
  },
  computed:{
    // SEO动态TDK参数
    seo_title(){
      return `${this.company.eng_brand_name} is recruiting on CareerInsights`
    },
    seo_keywords(){
      return `${this.company.eng_brand_name}, jobs in ${this.company.eng_brand_name}, Find more China company review in CareerInsights.net`
    },
    seo_description(){
      return `${this.company.introduce}`
    },
  }
  async asyncData({ context, query, params, store }) {
    const company = await getCompanyByBrandName({ brand_name: params.name });
    const companylist = await getCompanyList({ industry: company.industry });
    return {
      company: company.data,
      companylist: companylist.data,
    };
  },
```







职位详情

```javascript
  head(){
    return {
      title:  this.seo_title,
      meta:[
        {
          // hid: 'keywords',
          name:"keywords",
          content:this.seo_keywords
        },
        {
          // hid: 'description',
          name:"description",
          content:this.seo_description
        }
      ]
    }
  },
  computed:{
          // SEO动态TDK参数
    seo_title(){
      return `${this.job_detail.company.eng_brand_name} is recruiting  ${this.job_detail.position} in ${this.job_detail_city}- CareerInsights`
    },
    seo_keywords(){
      return `${this.job_detail.position}, ${this.job_detail.company.eng_brand_name}, looking for ${this.job_detail.position} jobs in ${this.job_detail_city}`
    },
    seo_description(){
      return `${this.job_detail.company.eng_brand_name} is recruiting ${this.job_detail.position} in ${this.job_detail_city}. the salary range is between ${this.job_detail.minimum_salary * 12} K and ${this.job_detail.maximum_salary * 12} K, the benefits contained  ${this.job_detail.benefits}. Find more ${this.job_detail.position} opportunities in China on CareerInsights.net`
    },
  }
  async asyncData({ store,context, query, params }) {
    let job_detail = await getJobInfoByJobID({
      job_id: params.job_id,
      user_id: 0,
    });
    var job_detail_city = []
    job_detail_city = store.state.config.work_location.filter(item=>{
      return item.value == job_detail.data.adcode && item.lang == 'en'
    })
    let job_similar = await getSimilarJobList({ job_id: params.job_id });
    let job_question = await getJobQuestion({ job_id: params.job_id });
    return {
      job_detail: job_detail.data,
      job_similar: job_similar.data,
      job_question: job_question.data,
      job_detail_city:  job_detail_city[0]?job_detail_city[0].city:'',
    };
  },
```

参考/扩展资料

```html
Nuxt的head方法
1.地址改成城市 2.title改成 3.hid
https://zh.nuxtjs.org/docs/2.x/configuration-glossary/configuration-head/
https://blog.csdn.net/awseda/article/details/106423386
https://www.oschina.net/question/4061903_2302336
```

