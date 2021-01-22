# Nuxt报错客户端的虚拟DOM渲染的内容与服务端的内容不匹配的解决

## 问题产生

在进行nuxt开发的时候,控制台出现了如下报错

```The client-side rendered virtual DOM tree is not matching server-rendered content```

## 思路

经过研究得知该报错是由于前端代码的注释未去掉造成的,将注释去掉后就不会出现这种报错了。

## 解决方式

去除el-cascader的注释即可

```html
      <div class="search-card">
        <div v-if="false" class="search">
          <div class="search-div">
            <svg
              style="margin: 13px 0px 16px 24px; float: left"
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                fill-rule="evenodd"
                clip-rule="evenodd"
                d="M16.0526 15.0857H17.1429L23.9863 21.9429L21.9429 23.9863L15.0857 17.1429V16.0594L14.7086 15.6823C13.152 17.0194 11.1291 17.8286 8.91429 17.8286C3.99086 17.8286 0 13.8377 0 8.91429C0 3.99086 3.99086 0 8.91429 0C13.8377 0 17.8286 3.99086 17.8286 8.91429C17.8286 11.1291 17.0194 13.152 15.6754 14.7086L16.0526 15.0857ZM2.74286 8.91429C2.74286 12.3223 5.50629 15.0857 8.91429 15.0857C12.3223 15.0857 15.0857 12.3223 15.0857 8.91429C15.0857 5.50629 12.3223 2.74286 8.91429 2.74286C5.50629 2.74286 2.74286 5.50629 2.74286 8.91429Z"
                fill="black"
              />
            </svg>
            <el-input
              class="search-input"
              v-model="search.keyword"
              @keyup.enter.native="getJobList"
              :placeholder="$t('jobs.jobs_search_bar.search_placeholder')"
            ></el-input>
            <!-- <el-cascader
            slot="prepend"
            clearable
            :options="work_locationList"
            v-model="search.work_location"
            placeholder="Location"
            class="search-input"
            style="width:300px;text-align:right;border-left:1px solid #ccc"
            filterable
          >
            </el-cascader>-->
            <div
              style="width: 300px; margin: 7px 0px; border-left: 1px solid #ccc"
            >
              <el-autocomplete
                v-model="work_location"
                :fetch-suggestions="querySearch"
                :placeholder="
                  $t('jobs.jobs_search_bar.location_search_placeholder')
                "
                @select="handleSelect"
                @keyup.enter.native="loactionSearch"
              >
                <template slot-scope="props">
                  <span class="addr">
                    {{ props.item.city }} , {{ props.item.province }}
                    <span v-if="props.item.country"
                      >, {{ props.item.country }}</span
                    >
                  </span>
                </template>
              </el-autocomplete>
            </div>
          </div>
```

## 参考扩展资料

```html
The client-side rendered virtual DOM tree is not matching server-rendered content
https://blog.csdn.net/weixin_42286528/article/details/93600201
https://www.baidu.com/s?ie=UTF-8&wd=The%20client-side%20rendered%20virtual%20DOM%20tree%20is%20not%20matching%20server-rendered%20content.%20This%20is%20likely%20caused%20by%20incorrect%20HTML%20markup,%20for%20example%20nesting%20block-level%20elements%20inside%20%3Cp%3E,%20or%20missing%20%3Ctbody%3E.%20Bailing%20hyd
```

