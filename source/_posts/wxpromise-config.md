---
title: -config基本配置
date: 2018-10-31 10:33:14
author: T.2stt
comments: true
tags:
- [wx小程序]
categories:
- [wx小程序]
---
>小程序的配置包括全局配置和界面配置
[全局配置](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#%E5%85%A8%E5%B1%80%E9%85%8D%E7%BD%AE)app.json：pages、window、tabBar、networkTimeout、debug、functionalPages、subpackages、workers、requiredBackgroundModes、plugins、preloadRule、resizable、navigateToMiniProgramAppIdList
[页面配置](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#%E9%A1%B5%E9%9D%A2%E9%85%8D%E7%BD%AE)xxx.json只能设置 window 相关的配置项，以决定本页面的窗口表现，所以无需写 window 这个键

# 全局配置
## pages

    数组的第一项代表小程序的初始页面（首页）。小程序中新增/减少页面，都需要对 pages 数组进行修改。
---

## window

    用于设置小程序的状态栏、导航条、标题、窗口背景色。
    ![例如](https://developers.weixin.qq.com/miniprogram/dev/image/config.jpg?t=18103009)
---

## tabBar

    如果小程序是一个多 tab 应用（客户端窗口的底部或顶部有 tab 栏可以切换页面），可以通过 tabBar 配置项指定 tab 栏的表现，以及 tab 切换时显示的对应页面。
    ![例子](https://developers.weixin.qq.com/miniprogram/dev/image/tabbar.png?t=18103009)
---

## networkTimeout
    各类网络请求的超时时间，单位均为毫秒。(request,connectSocket,uploadFile,downloadFile)
---

## debug
    调试信息以 info 的形式给出，其信息有Page的注册，页面路由，数据更新，事件触发等。可以帮助开发者快速定位一些常见的问题。
---

## functionalPages
支持某些接口不能在插件中直接调用
- 获取用户信息
- 支付
[插件功能页](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/functional-pages.html)

---
## subpackages
在构建时打包成不同的分包，用户在使用时<font color="red">按需进行加载</font>。
[分包加载的使用](https://developers.weixin.qq.com/miniprogram/dev/framework/subpackages.html)
[使用分包](https://developers.weixin.qq.com/miniprogram/dev/framework/subpackages/basic.html)
[独立分包,特殊类型的分包,独立分包不依赖主包即可运行](https://developers.weixin.qq.com/miniprogram/dev/framework/subpackages/independent.html)
[分包预下载](https://developers.weixin.qq.com/miniprogram/dev/framework/subpackages/preload.html)

---
## worker
使用 Worker 处理多线程任务时，设置 Worker 代码放置的目录
[多线程 Worker](https://developers.weixin.qq.com/miniprogram/dev/framework/workers.html)

---
## requiredBackgroundModes
申明需要后台运行的能力，类型为数组。目前支持以下项目：
- audio: 后台音乐播放
```bash
{
  "pages": ["pages/index/index"],
  "requiredBackgroundModes": ["audio"]
}
```

---

## plugins
声明小程序需要[使用的插件。](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/using.html)

---

## preloadRule
[声明分包预下载的规则。](https://developers.weixin.qq.com/miniprogram/dev/framework/subpackages/preload.html)

---

## resizable
在 iPad 上运行的小程序可以设置支持屏幕旋转。[屏幕响应显示区域变化](https://developers.weixin.qq.com/miniprogram/dev/framework/view/resizable.html)

---
## navigateToMiniProgramAppIdList
当小程序需要使用 wx.navigateToMiniProgram 接口跳转到其他小程序时，需要先在配置文件中声明需要跳转的小程序 appId 列表，最多允许填写 10 个。
[wx.navigateToMiniProgram(Object object)](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/miniprogram-navigate/wx.navigateToMiniProgram.html)

# 页面配置
    每一个小程序页面也可以使用.json文件来对本页面的窗口表现进行配置。
    页面的配置只能设置 app.json 中部分 window 配置项的内容，页面中配置项会覆盖 app.json 的 window 中相同的配置项。以决定本页面的窗口表现，所以无需写 window 这个键。
[demo](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#%E9%A1%B5%E9%9D%A2%E9%85%8D%E7%BD%AE)

## navigationBarBackgroundColor
导航栏背景颜色

---

## navigationBarTextStyle
    导航栏标题颜色，仅支持 black / white
---
## navigationBarTitleText
    导航栏标题文字内容
---
## backgroundColor
    窗口的背景色
---

## backgroundTextStyle
    下拉 loading 的样式，仅支持 dark / light
---

## enablePullDownRefresh
    是否全局开启下拉刷新。
---
## onReachBottomDistance
    页面上拉触底事件触发时距页面底部距离，单位为px。
---
## backgroundColor
    窗口的背景色
---
## disableScroll
    设置为 true 则页面整体不能上下滚动；只在页面配置中有效，无法在 app.json 中设置该项
---
## 组件化的使用 usingComponents
该字段是对引入的组件进行导入
<font color="red">【优点】</font>
将其js、wxml、wxss都进行使用（demo模仿拼多多）
```bash
"usingComponents": {
    "slideTab": "../../components/navigator/index",
    "productBox":"../../components/product_box/index",
    "swiperImg":"../../components/swiper_img/index",
    "loadmore":"../../components/loadmore/index"
},
```