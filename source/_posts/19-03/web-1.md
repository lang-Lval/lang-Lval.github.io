---
title: 判断设备类型
tags: web
categories: web
date: 2019-03-19 18:08:55
---

### 判断iPhone X

```
// 判断iPhone X

function isIphoneX(){
    return /iphone/gi.test(navigator.userAgent)&&(screen.height == 812 && screen.width==375)
}

$(function(){
    if(isIphoneX()){
        $('html').addClass('iphonex')
    }
})


如果只是改变样式,在.iPhone X类名下接着写类名+样式即可
.iphonex .wp .middle .middle_text {
    padding-top: 8.925rem;
}
.iphonex .s2_content {
    margin-top: 14.875rem;
}
```
<!-- more-->

### 判断iOS或安卓

```
//判断iOS或安卓

$(function () {
    var u = navigator.userAgent, app = navigator.appVersion;
    var isAndroid = u.indexOf('Android') > -1 || u.indexOf('Linux') > -1; //g
    var isIOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
    if (isAndroid) {
        alert("安卓机！")
    }
    if (isIOS) {
        alert("苹果果机！")
    }
});

```

### 判断PC端与WAP端

```
var mobile_bs = {
 versions: function() {
  var u = navigator.userAgent;
  return {
   trident: u.indexOf('Trident') > -1, //IE内核
   presto: u.indexOf('Presto') > -1, //opera内核
   webKit: u.indexOf('AppleWebKit') > -1, //苹果、谷歌内核
   gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1, //火狐内核
   mobile: !! u.match(/AppleWebKit.*Mobile.*/) || !! u.match(/AppleWebKit/) && u.indexOf('QIHU') && u.indexOf('QIHU') > -1 && u.indexOf('Chrome') < 0, //是否为移动终端
   ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
   android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, //android终端或者uc浏览器
   iPhone: u.indexOf('iPhone') > -1 || u.indexOf('Mac') > -1, //是否为iPhone或者QQHD浏览器
   iPad: u.indexOf('iPad') > -1,  //是否iPad
   webApp: u.indexOf('Safari') == -1 //是否web应该程序，没有头部与底部
  }
 } ()
};
if (mobile_bs.versions.mobile) {
 if (mobile_bs.versions.android || mobile_bs.versions.iPhone || mobile_bs.versions.iPad || mobile_bs.versions.ios) {
  window.location.href = "移动端网址";
 }
};
```


### js判断当前页面在移动设备还是在PC端中打开


//js判断当前页面在移动设备还是在PC端中打开

```
 var browser = {
              versions: function () {
                var u = navigator.userAgent, app = navigator.appVersion;
                return {     //移动终端浏览器版本信息
                  trident: u.indexOf('Trident') > -1, //IE内核
                  presto: u.indexOf('Presto') > -1, //opera内核
                  webKit: u.indexOf('AppleWebKit') > -1, //苹果、谷歌内核
                  gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1, //火狐内核
                  mobile: !!u.match(/AppleWebKit.*Mobile.*/), //是否为移动终端
                  ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
                  android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, //android终端或uc浏览器
                  iPhone: u.indexOf('iPhone') > -1, //是否为iPhone或者QQHD浏览器
                  iPad: u.indexOf('iPad') > -1, //是否iPad
                  webApp: u.indexOf('Safari') == -1 //是否web应该程序，没有头部与底部
                };
              }(),
              language: (navigator.browserLanguage || navigator.language).toLowerCase()
            }            
            if (browser.versions.mobile) {//判断是否是移动设备打开。browser代码在下面
                var ua = navigator.userAgent.toLowerCase();//获取判断用的对象
                if (ua.match(/MicroMessenger/i) == "micromessenger") {
                    //在微信中打开
                   setInterval(WeixinJSBridge.call('closeWindow'),2000);
                }
                if (ua.match(/WeiBo/i) == "weibo") {
                    //在新浪微博客户端打开
                }
                if (ua.match(/QQ/i) == "qq") {
                    //在QQ空间打开
                }
                if (browser.versions.ios) {
                    //是否在IOS浏览器打开
                } 
                if(browser.versions.android){
                    //是否在安卓浏览器打开
                }
            } else {
                //否则就是PC浏览器打开
                window.close();
            }
```


### js判断是否为电脑端

```
//js判断是否为电脑端


//如果返回的是false说明当前操作系统是手机端，如果返回的是true则说明当前的操作系统是电脑端
function IsPC() {
    var userAgentInfo = navigator.userAgent;
    var Agents = ["Android", "iPhone",
        "SymbianOS", "Windows Phone",
        "iPad", "iPod"];
    var flag = true;
    for (var v = 0; v < Agents.length; v++) {
        if (userAgentInfo.indexOf(Agents[v]) > 0) {
            flag = false;
            break;
        }
    }
    return flag;
}
```

