## Nuxt引入QRCode.js的步骤

## 问题

在迁移vue-cli至nuxt的过程中,按照原本的方式引入使用qrcode.js会报错document is not defined,如何才能在nuxt中正确使用Qrcode.js呢？

## 思路

(1)在nuxt的首页文件app.html通过script标签的形式引入qrcode.js文件

```html
<!DOCTYPE html>
<html {{ HTML_ATTRS }}>
<script src="https://bicuz.com/qrcode.js"></script>

<head {{ HEAD_ATTRS }}>
    {{ HEAD }}
    <meta charset="utf-8" />

    <meta name="baidu-site-verification" content="TryUHyXa8Y" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no" />
    <title>
        CareerInsights
    </title>

    <meta name="keywords" content="招聘外国人,招聘老外,外籍人才,招聘外籍人才,外籍人才求职,招聘外教,国际人才,上海外国人,北京外国人,深圳外国人,广州外国人,外国人就业,外籍人才招聘网站" />
    <meta name="keywords" content="find Job in China,Working in China,find job in Shenzhen, find job in shanghai, find job in beijing, find job in china, find job in Guangzhou" />
    <meta name="description" content="CareerInsights是一家专业的外籍人才招聘平台。企业能快速通过平台，发布、更新招聘信息，与求职者直接沟通。同时，平台上所有发布职位企业均是通过认证企业，大大提高外籍人才就业安全性。CareerInsights is a recruitment platform in connecting job opportunities in China with foreign talents." />

    <meta name="author" content="CareerInsights" />
    <meta name="theme-color" content="#367dbe" />
    <meta name="subject" content="专业的外籍人才招聘网站,Post Job For Free" />

    <meta name="robots" content="index,follow,noodp" />
    <meta name="googlebot" content="index,follow,noodp" />
    <meta name="baidu-site-verification" content="TryUHyXa8Y" />

    <link rel="icon" type="image/x-icon" href="https://www.careerinsights.net/favicon.ico" />
</head>


<!-- <link rel="stylesheet" href="//at.alicdn.com/t/font_1180806_k64nbgmu96e.css"> -->
<!-- <script src="https://polyfill.io/v3/polyfill.min.js"></script> -->

<script type="text/javascript" src="https://res2.wx.qq.com/open/js/jweixin-1.6.0.js"></script>
<script type="text/javascript" src="https://webapi.amap.com/maps?v=1.4.15&key=fe0ad8c6246b630d3a76e07257643f04&plugin=AMap.Driving"></script>
<script>
    var _hmt = _hmt || [];
    (function() {
        var hm = document.createElement("script");
        hm.src = "https://hm.baidu.com/hm.js?02f14a94c2d707befd7588c6223d8529";
        var s = document.getElementsByTagName("script")[0];
        s.parentNode.insertBefore(hm, s);
    })();
</script>
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-163250573-1"></script>
<script>
    window.dataLayer = window.dataLayer || [];

    function gtag() {
        dataLayer.push(arguments);
    }
    gtag('js', new Date());
    gtag('config', 'UA-163250573-1');
</script>
<script>
    // console.log = function(){};
</script>
<!-- <script type="text/javascript" src="index.js"></script> -->

<body {{ BODY_ATTRS }}>
    {{ APP }}
</body>

</html>
```

(2)创建生成qrcode的函数

```javascript
<div class="qrcode" ref="qrCodeUrl"></div>

creatQrCode() {
      var redirect_uri = window.location.href;
      var qrcode = new QRCode(this.$refs.qrCodeUrl, {
        text: redirect_uri,
        width: 130,
        height: 130,
        colorDark: "#000000",
        colorLight: "#ffffff",
        correctLevel: QRCode.CorrectLevel.H
      });
    },
```

(3)页面初始化的时候通过thenextTick函数调用生成二维码的函数(解决直接调用creatQrCode依旧报错document is not defined的问题)

```javascript
mounted(){
	this.getResume();
}
getResume() {
      getResumeByUserID({
        user_id: this.userinfo.user_id
      }).then(response => {
        if (response.success && response.data) {
          this.resume = response.data;
          this.$nextTick(function () {
            this.creatQrCode();
          });
        }
      });
 },
```

## 参考/扩展资料

```html
nuxt使用QRCode.js 生成二维码 解决方案  document is not defined
https://blog.csdn.net/weixin_30477797/article/details/95474212
https://blog.csdn.net/qq_21363577/article/details/87899582
https://www.cnblogs.com/djjlovedjj/archive/2004/01/13/10283131.html
```

