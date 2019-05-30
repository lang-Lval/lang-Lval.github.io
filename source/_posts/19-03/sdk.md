---
title: 微信jssdk的接入（分享及扫码功能）
tags: vue
categories: vue
date: 2019-03-01 15:43:06
---
![avatar](https://raw.githubusercontent.com/langhuonan/wechat/master/mdImages/day1/1.jpg)
### vue中使用（vux版）
#### 微信扫一扫功能
分享接口只有认证公众号才能使用，域名必须备案且在微信后台设置。
<!--more-->
先确认已经满足使用jssdk的要求再进行开发。
1.引入vux类库
2.在 main.js 中全局引入
```
import { WechatPlugin } from 'vux'
Vue.use(WechatPlugin)

console.log(Vue.wechat) // 可以直接访问 wx 对象。
```
3.全局引入后就可以在任意组件内调用了，如：
```
this.$wechat.scanQRCode({
		needResult: 0, // 默认为0，扫描结果由微信处理，1则直接返回扫描结果，
		scanType: ["qrCode","barCode"], // 可以指定扫二维码还是一维码，默认二者都有
		success: function (res) {
		var result = res.resultStr; // 当needResult 为 1 时，扫码返回的结果
		}			
	});
```
#### 准备工作完成，下面开始正式的流程

步骤一、绑定域名
先登录微信公众平台进入“公众号设置”的“功能设置”里填写“JS接口安全域名” （这个一般是后台去做的）

步骤二、引入JS文件
jquery的项目在需要调用JS接口的页面引入如下JS文件，（支持https）：http://res.wx.qq.com/open/js/jweixin-1.4.0.js
vue的项目可以用vux的组件，就是我上面介绍的那一堆东西

步骤三、通过config接口注入权限验证配置
这个过程一般都是在App.vue中完成
思路：先向后台请求配置所需要的数据，也就是下面的scan()函数,得到数据后在通过config接口注入权限验证配置，也就是init()函数，最后在create生命周期中调用一下scan()函数，这一步就算完成了
```
init:function(data){
		  //通过config接口注入权限验证配置
		  this.$wechat.config({
			  debug: false, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
			  appId: data.app_id, // 必填，公众号的唯一标识
			  timestamp: data.timestamp, // 必填，生成签名的时间戳
			  nonceStr: data.nonceStr, // 必填，生成签名的随机串
			  signature: data.signature,// 必填，签名，见附录1
			  jsApiList: ['scanQRCode'] // 必填，需要使用的JS接口列表
		  });
		  //通过ready接口处理成功验证

	  },
scan:function(){
		  this.$http({
			  methods:'get',
			  url:'/gongzhonghao/Jssdk',
			  params:{
				  // 代码需要上传服务器，否则返回为0
				  url:location.href.split('#')[0],
			  }
		  }).then(res=>{
			  this.init(res.data.data)
		  })
	  },
```
步骤四、在需要的组件内调用功能接口，如下面调用的是扫一扫的接口
```
let self = this;
self.$wechat.scanQRCode({
		needResult: 1, // 默认为0，扫描结果由微信处理，1则直接返回扫描结果，
		scanType: ["qrCode"], // 可以指定扫二维码还是一维码，默认二者都有
		success: function (res) {
			self.$vux.loading.hide();
			var result = res.resultStr; // 当needResult 为 1 时，扫码返回的结果
			if (result){
				self.$store.commit('setPrizeQrCode',result);
				self.getPrizeQrcodeInfo(result)
			}else {
				self.$vux.alert.show({
					content:'未识别的兑换码'
				})
			}

		}
	});
```
注意：微信的sdk只能在微信中打开，如果不是，则无法使用微信的功能，下面是判断是否微信打开的函数
```
isWeixn:function(){
		var ua = navigator.userAgent.toLowerCase();
		if(ua.match(/MicroMessenger/i)=="micromessenger") {
			return true;
		} else {
			return false;
		}
	},
	//判断是否微信的处理
	if(this.isWexin()){
		//是微信，在里面写相关的业务逻辑，如分享，扫码等功能
	}else {
		//这个是硬性不支持，没有办法的
		alert('请在微信中打开')
	}
```


附录：[微信js-sdk说明文档](https://mp.weixin.qq.com/wiki?)

### jquery中使用sdk
1.在需要调用JS接口的页面引入如下JS文件，（支持https）：http://res.wx.qq.com/open/js/jweixin-1.4.0.js

2.jssdk的签名权限,这个权限是由后台提供的,前端只需要把签名权限注入到wx.config中就可以了

```
//初始化jssdk
    var init = function(data){
        //通过config接口注入权限验证配置
        wx.config({
            debug: false, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
            appId: data.app_id, // 必填，公众号的唯一标识
            timestamp: data.timestamp, // 必填，生成签名的时间戳
            nonceStr: data.nonceStr, // 必填，生成签名的随机串
            signature: data.signature,// 必填，签名，见附录1
            jsApiList: ['onMenuShareTimeline','onMenuShareAppMessage'] // 必填，需要使用的JS接口列表
        });
        //通过ready接口处理成功验证
        wx.ready(function(){
            //分享到朋友圈
            
//  		var local_url = $.getStorge('auth_url');
//  		if(!local_url){
//  			local_url = window.location.href;
//  		}
    		var local_url = window.location.href;
           
            console.log(local_url);
            wx.onMenuShareTimeline({
			    title: desc+'--'+title, // 分享标题
			    link: local_url, // 分享链接，该链接域名或路径必须与当前页面对应的公众号JS安全域名一致
			    imgUrl: 'http://jssdk.cloud-cy.com/wechatShopServer/'+logo, // 分享图标
			    success: function () { 
			        // 用户确认分享后执行的回调函数
			    },
			    cancel: function () { 
			        // 用户取消分享后执行的回调函数
			    }
			});
			//分享给朋友
			wx.onMenuShareAppMessage({
			    title: title, // 分享标题
			    desc: desc, // 分享描述
			    link: local_url, // 分享链接，该链接域名或路径必须与当前页面对应的公众号JS安全域名一致
			    imgUrl: 'http://jssdk.cloud-cy.com/wechatShopServer/'+logo, // 分享图标
			    type: 'link', // 分享类型,music、video或link，不填默认为link
			    dataUrl: '', // 如果type是music或video，则要提供数据链接，默认为空
			    success: function () { 
			        // 用户确认分享后执行的回调函数
			    },
			    cancel: function () { 
			        // 用户取消分享后执行的回调函数
			    }
			});
        });
    }
	//获取jssdk配置
	$.ajax({
        type:"get",
        url:web+"/Jssdk",
        data:{
        	url:window.location.href,
        	url_code:account
        },
        dataType:"json",
        success:function(data,textStatus){console.log(data);
            if(data.code==200){
                init(data.data);
            }else{
//              alert("连接失败，请稍后再试或联系管理员");
            }

        }
	});
	
```

