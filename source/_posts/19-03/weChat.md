---
title: 微信小程序基础
date: 2019-02-28 15:16:13
categories: 
- 小程序
---
![avatar](https://raw.githubusercontent.com/langhuonan/wechat/master/mdImages/day3/wechat.jpg)

## 微信小程序相关及一些小坑

### 1.页面滑动
小程序内容超出父盒子，或者页面超出屏幕都不会出现滚动条滑动，如需要滑动，要使用scroll-view组件包裹着要滑动的区域
[scroll-view](https://developers.weixin.qq.com/miniprogram/dev/component/scroll-view.html)相关配置属性
<!-- more -->
#### scroll-view相关
1. scroll-view生效需要设置高度，如果没有设置高度，滑动效果不生效
2. 去除scroll-view默认滚动条
```
::-webkit-scrollbar{

width: 0;

height: 0;

color: transparent;

}
```
### 2.闭包问题
请求后台api中的success函数实际是一个闭包 ， 无法直接通过this来设置setData;
解决方案：
1.将当前对象赋给一个新对象：let that = this
2.使用箭头函数
```
  onLoad: function (options) {
    console.log(options.uselogid)
    wx.request({
      url: util.Api.getUserPrizeInfo,
      method: 'get',
      data: {
        openid: app.globalData.openid,
        user_log_id: options.uselogid
      },
      success: (res) => {
        // console.log(res.data)
        this.setData({
          cardData: res.data.data,
          qrCode: res.data.data.qrCodeUrl,
          uselogid: options.uselogid,
          status: res.data.data.status
        })
        console.log(this.data.status)
      }
    })
  }
  ```
  ### 3.背景图片问题
  小程序中的背景图片写在wxss预览时无法显现
  解决方案：
  1.将图片转为base64格式，才能在wxss内设置
  2.写行内样式，图片用网络图片，本地图片无法预览
 
 ### 4.路由
 1.使用navigator标签导航，注意url地址不能带文件后缀
 2.在js文件内注册事件跳转
 wx.navigateTo跳转保留当前页面，也就是说会有返回箭头
 ```
  wx.navigateTo({
        url: '/pages/cardDetial/cardDetial?uselogid=' + e.currentTarget.dataset.uselogid,
      })
```
wx.redirectTo跳转 关闭当前页面，跳转到应用内的某个页面。但是不允许跳转到 tabbar 页面。
```
wx.redirectTo({
      url: '/pages/superGift/superGift',
    })
```
### 5.页面间传值
传值:和web页面差不多，也是在url地址后面拼接参数
```
wx.navigateTo({
        url: '/pages/cardDetial/cardDetial?uselogid=' + e.currentTarget.dataset.uselogid,
      })
```
接收：直接在生命周期onLoad里用参数接收
```
  onLoad: function (options) {
    console.log(options.uselogid)
  },
```

### 6.无法使用外部字体图标
大部分情况下直接下载iconfont的压缩包，将其内的iconfont.css文件复制到一个新的wxss文件内，然后在app.wxss文件内全局引入@import "/lib/style/iconfont.wxss";就可以在任意文件中使用了
如果出现无法使用的情况，试试下面方法：
1，下载font-awesome字体包

2，打开Transfonter网站，上传字体iconfont.ttf，选择base64编码

3，convert完毕后点击下载，下载得到的包中有stylesheet.css文件，打开并对照font-awesome.css中的内容进行合并base64部分，加入到微信小程序的xxx.wxss文件中进行使用

    <text class="iconfont icon-xxxx"></text>

### 7.setData修改数组或对象
setData无法直接修改引用类型的数据，需要字符串拼接的方式保存到变量中
```
 var closeShow = "redPacket.myClosePackect"
 self.setData({
             
              [closeShow]: false,
              
            })
```

### 8.textarea注意事项
1.bug: 微信版本 6.3.30，textarea 在列表渲染时，新增加的 textarea 在自动聚焦时的位置计算错误。
2.tip: textarea 的 blur 事件会晚于页面上的 tap 事件，如果需要在 button 的点击事件获取 textarea，可以使用 form 的 bindsubmit。
3.tip: 不建议在多行文本上对用户的输入进行修改，所以 textarea 的 bindinput 处理函数并不会将返回值反映到 textarea 上。
4.tip: textarea 组件是由客户端创建的原生组件，它的层级是最高的，不能通过 z-index 控制层级。
5.tip: 请勿在 scroll-view、swiper、picker-view、movable-view 中使用 textarea 组件。
6.tip: css 动画对 textarea 组件无效。

### 9.原生组件无法被覆盖的问题
map、video、canvas、camera 等原生组件上不可被标签覆盖，如果需要覆盖，要使用cover-view标签 且只支持嵌套cover-view、cover-image