---
title: 基于Vue框架的微信网页进行微信支付
date: 2020-10-15 11:01:59
tags: [微信,教程]
categories: 教程
keywords: 微信支付
top_img: https://oss.fycoder.top/Blog/wechatPayTopPic.jpeg
cover: https://oss.fycoder.top/Blog/wechatPayTopPic.jpeg
---
# 导语

作为一个先接触微信小程序再做微信公众号的前端萌新，第一次写微信网页的微信支付，感觉步骤有点繁琐，写篇博客帮助一下有困惑的小伙伴，也防止以后忘记
要实现公众号微信支付，最基础的是拥有一个微信服务号、一个微信商户平台账号、一个服务器和域名，且微信服务号绑定了微信商户平台，这些步骤就不细说了

#  一、微信后台配置接口
##  1.配置微信支付的合法地址
登录微信商户平台，进入产品中心——>开发配置，设置JSAPI支付授权目录，输入你的服务器域名和文件目录，然后输入密码和验证码就可以配置成功。
然后进入账户中心——>API安全——>API密钥
这里需要配置一个给后台的秘钥（这个应该主要是后台考虑的问题），32位数字字母的秘钥，要保存好，忘了只能重设。
##  2.配置JS接口安全域名
这次要登录微信公众平台，登录你的服务号，进入公众号设置——>功能设置，设置JS接口安全域名，JS接口安全域名是确保你可以在这个网页调用微信的SDK的
设置这个的时候要在服务器你填的目录下放一个微信提供的txt文件，要和后端一起合作完成
#  二、安装微信SDK依赖
##  1.安装

```
npm install weixin-jsapi
```
##  2.在需要的页面导入
```
import wx from "weixin-jsapi"
```
#  三、在页面中配置SDK
微信SDK在每个需要使用的页面都要进行配置，直接在页面创建的时候配置
```
created() {
     //获取当前页面url
    let url = location.href
    //调用后端接口，给后台url，让后台返回appId、时间戳、随机串、签名，建议vue路由使用hash模式，可以根据#分隔路由
    getJsSDKInfo({ticketUrl: url.split("#")[0]}).then(res => {
      console.log(res)
      wx.config({
        debug: false,//用于调试，这里一般在测试阶段先用true，等打包给后台的时候就改回false,
        appId: res.data.result.appId,
        timestamp: res.data.result.timestamp,
        nonceStr: res.data.result.nonceStr,
        signature: res.data.result.signature,
        jsApiList: ['chooseWXPay']//需要使用的API，此处只使用了微信支付接口
      })
      //查看配置是否成功，成功时候才能使用微信SDK
      wx.ready(() => {
        wx.checkJsApi({
          jsApiList: ['chooseWXPay'],
          success: function (res) {
            console.log("success")
            console.log(res)
          },
          fail: function (res) {
            console.log("fail");
            console.log(res)
          }
        })
      })
    })
  },

```
在成功配置之后，我们就可以在需要的地方调用微信支付的接口了
#  四、使用微信支付接口
```
    pay() {//监听需要支付的按钮，调用该函数
      //调用后台接口，获取时间戳、字符串、签名方式、支付签名和package
      getPayInfo({reportId: this.resultId}).then(res=>{
        console.log(res)
        wx.chooseWXPay({
          timestamp: res.data.result.timeStamp, // 支付签名时间戳，注意微信jssdk中的所有使用timestamp字段均为小写。但最新版的支付后台生成签名使用的timeStamp字段名需大写其中的S字符
          nonceStr: res.data.result.nonceStr, // 支付签名随机串，不长于 32 位
          package: res.data.result.package, // 统一支付接口返回的prepay_id参数值，提交格式如：prepay_id=\*\*\*）
          signType: res.data.result.signType, // 签名方式，默认为'SHA1'，使用新版支付需传入'MD5'
          paySign: res.data.result.paySign, // 支付签名
          success:  (res)=> {//成功回调函数
            console.log("success")
            getReportDetails()//支付成功之后需要做的事
          }
        })
      })
    }
```