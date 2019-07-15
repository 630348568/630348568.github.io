---
title: ajax、axios、fetch区别
date: 2019-02-27 12:19:00
author: T.2stt
comments: true
tags:
- js
categories:
- [js,ms]
---
>将jQuery的ajax、axios和fetch做个简单的比较
# jQuery ajax
## 定义
**Ajax** = 异步 `JavaScript` 和 `XML` 或者是 `HTML`（标准通用标记语言的子集）。
**Ajax** 是一种用于创建快速`动态网页的技术`。
**Ajax** 是一种在`无需重新加载整个网页的情况下，能够更新部分网页的技术`。

```javascript
$.ajax({
   type: 'POST',
   url: url,
   data: data,
   dataType: dataType,
   success: function () {},
   error: function () {}
})
```
## 优缺点
- 本身是针对 `MVC` 的编程,不符合现在前端 `MVVM` 的浪潮;
- 基于原生的XHR开发，XHR本身的架构不清晰，已经有了fetch的替代方案;
- JQ 整个项目太大，单纯使用 `ajax` 却要引入整个 JQ 非常的不合理（采取个性化打包的方案又不能享受CDN服务）;

# axios
基于``promise` 用于浏览器 和 node.js 的 **http客户端**
## 特点
- 支持浏览器 和 node.js;
- 支持 `Promise`;
- 能拦截请求和响应;
- 能转换请求和响应数据;
- 能取消请求。`axios` 的 `config` 中提供了一个 `cancelToken` 属性，可以通过传递一个新的 `CancelToken` 对象来在请求的任何阶段关闭请求[demo](https://www.cnblogs.com/hahazexia/p/10001782.html)
- 自动转换 JSON 数据;
- 浏览器端支持防止CSRF(跨站请求伪造);
```javascript
axios({
    method: 'post',
    url: '/user/12345',
    data: {
        firstName: 'Fred',
        lastName: 'Flintstone'
    }
})
.then(function (response) {
    console.log(response);
})
.catch(function (error) {
    console.log(error);
});
```
## 请求方法别名
axios.request(config)
axios.get(url[, config])
axios.delete(url[, config])
axios.head(url[, config])
axios.options(url[, config])
axios.post(url[, data[, config]])
axios.put(url[, data[, config]])
axios.patch(url[, data[, config]])

## 优缺点
- 从 node.js 创建 http 请求;
- 支持 Promise API;
- 客户端支持防止CSRF;
- 提供了一些并发请求的接口（重要，方便了很多的操作）;

# fetch
```javascript
try {
  let response = await fetch(url);
  let data = response.json();
  console.log(data);
} catch(e) {
  console.log("Oops, error", e);
}
```
<font color="red">优缺点</font>
- 符合关注分离，没有将输入、输出和用事件来跟踪的状态混杂在一个对象里
- 更好更方便的写法
- 更加底层，提供的API丰富（request, response）
- 脱离了XHR，是ES规范里新的实现方式
**1）** fetchtch只对网络请求报错，对400，500都当做成功的请求，需要封装去处理
**2）**fetch默认不会带cookie，需要添加配置项
**3）**fetch不支持abort，不支持超时控制，使用setTimeout及Promise.reject的实现的超时控制并不能阻止请求过程继续在后台运行，造成了量的浪费
**4）**fetch没有办法原生监测请求的进度，而XHR可以

# flyio
原生小程序的 `request` 请求都是异步请求，使用起来很麻烦，而 **flyio** 不仅可以实现 `Promise` ，而且还可以对所有的页面请求进行拦截，使用起来非常的方便。
目前Fly.js支持的平台包括：Node.js 、微信小程序 、Weex 、React Native 、Quick App 和浏览器
## 特点
- 提供统一的 `Promise API`；
- 浏览器环境下，轻量且非常轻量 ；
- 支持多种JavaScript 运行环境；
- 支持请求／响应拦截器；
- 自动转换 JSON 数据；
- 支持切换底层 Http Engine，可轻松适配各种运行环境；
- 浏览器端支持全局Ajax拦截 ；
- H5页面内嵌到原生 APP 中时，支持将 http 请求转发到 Native。支持直接请求图片；

## 例子
```javascript
var Fly = require("flyio.js")//引入路径根据自己放置的目录配置
var fly = new Fly;
fly.interceptors.request.use((request) => {
    request.timeout = 30000;
    if (wx.getStorageSync('token')) {//检查本地缓存是否有token存在没有则重新获取
        request.headers = {//设置请求头
            "content-type": "application/json",
            "cld.stats.page_entry": wx.getStorageSync('scene'),
            "version": app.globalData.version,
            "token": wx.getStorageSync('token')
        }
        return request;
    } else {
        fly.lock();//锁住请求
        return app.Load().then(res => {//重新获取token
            request.timeout = 30000,
                request.headers = {//设置请求头
                    "content-type": "application/json",
                    "cld.stats.page_entry": wx.getStorageSync('scene'),
                    "version": app.globalData.version,
                    "token": wx.getStorageSync('token')
                }
            wx.showLoading({
                title: "加载中",
                mask: true,
            });
            fly.unlock();//解锁请求
            return request;//继续之前的请求
        })
    }
})
fly.interceptors.response.use(
    (response) => {
        wx.hideLoading();
        return response;//请求成功之后将返回值返回
    },
    (err) => {
        //请求出错，根据返回状态码判断出错原因
        console.log(err)
        wx.hideLoading();
        if (err.status == 0) {
            return "网络连接异常"
        } else if (err.status == 1) {
            return "网络连接超时"
        } else if (err.status == 401) {
            return "用户未登录"
        } else {
            if (err.response.data.message) {
                return err.response.data.message
            } else {
                return '请求数据失败,请稍后再试'
            }
        };
        // Do something with response error
    }
)
export default {
    fly: fly
}
```
在其他页面引用
```javascript
import http from "../../utils/http.js"
```
使用：
```javascript
http.fly.post(baseUrl + 'tt/news', {
    type: 1,
    page: 1
}).then(res => {
    console.log(res)
})
```
## mpvue中自己用fly
```javascript
var Fly = require('flyio/dist/npm/wx') // npm引入方式
const request = new Fly()
request.config.timeout = 10 * 1000
let isOk = true
// request.config.baseURL = 'http://192.168.1.85:8070'
request.config.baseURL = 'https://cjbeta.dhchecheng.com/api/'
request.interceptors.request.use((request) => {
  // 给所有请求添加自定义header
  wx.showLoading({
    title: '加载中',
    mask: true
  })
  wx.showNavigationBarLoading()
  return request
})
request.interceptors.response.use(
  (response, promise) => {
    let resCode = response.data.code
    wx.hideLoading()
    wx.hideNavigationBarLoading()
    if (resCode !== '200') {
      if (response.data.message) {
        wx.showToast({
          title: response.data.message,
          icon: 'none'
        })
      }
      if (response.data.code === '421') {
        // eslint-disable-next-line
        let inviteId = getCurrentPages().pop().data.$root[0].inviteId
        let goLoginFn = function () {
          isOk = false
          wx.getSetting({
            success: function (resGetSetting) {
              if (resGetSetting.authSetting['scope.userInfo']) {
                wx.getUserInfo({
                  success: function (resGetUserInfo) {
                    let userInfo = resGetUserInfo.userInfo
                    let openId = wx.getStorageSync('openId')
                    let postData = {
                      openId: openId,
                      unionId: '',
                      nickName: userInfo.nickName,
                      gender: userInfo.gender,
                      avatarUrl: userInfo.avatarUrl,
                      province: userInfo.province,
                      city: userInfo.city,
                      country: userInfo.country,
                      inviteId: inviteId
                    }
                    request.post('wechat/login', postData).then(res => {
                      wx.setStorage({
                        key: 'userInfo',
                        data: res.data,
                        success () {
                          isOk = true
                          request.config.headers.token = res.data.token
                          // eslint-disable-next-line
                          if (getCurrentPages().pop()) {
                            // eslint-disable-next-line
                            getCurrentPages().pop().onPullDownRefresh()
                          }
                        }
                      })
                    })
                  }
                })
              } else {
                // eslint-disable-next-line
                let shareUrl = '/' + getCurrentPages().pop().route
                // eslint-disable-next-line
                let options = getCurrentPages().pop().options
                let sn_ = ''
                if (shareUrl.indexOf('pages/prize_detail/prize_detail') !== -1) {
                  if (options.sn || options.scene) {
                    sn_ = '&sn=' + (options.sn || options.scene)
                  }
                }
                wx.navigateTo({
                  url: '/pages/authorize/main?shareUrl=' + shareUrl + sn_ + '&inviteId=' + inviteId
                })
              }
            }
          })
        }
        if (!isOk) {
          return
        }
        goLoginFn()
        return
      }
      wx.showToast({
        title: response.data.message || '服务器异常，请稍后重试',
        icon: 'none'
      })
    }
    return promise.resolve(response.data)
  },
  (err, promise) => {
    // 状态为0 网络错误
    if (Number(err.status) === 0) {
      wx.showToast({
        title: '网络错误,请稍后重试',
        icon: 'none'
      })
      return promise.resolve()
    }
    // 状态为1 网络超时
    if (Number(err.status) === 1) {
      wx.showToast({
        title: '网络超时，请稍后重试',
        icon: 'none'
      })
      return promise.resolve()
    }
    if (Number(err.status) > 200) {
      wx.showToast({
        title: err.status + '！服务器异常,请稍后重试',
        icon: 'none'
      })
      return promise.resolve()
    }
    // wx.hideLoading()
    wx.showToast({
      title: err.message,
      icon: 'none'
    })
    return promise.resolve()
  }
)
export default request

```
在`main.js`中加入
```javascript
import request from './utils/flyPlugin'
Vue.prototype.request = request
```
使用
```javascript
async getListFn () {
    let ordersList = await this.request.get('orders/list', {page: SelectData.isShowNoDataImg] = ordersList.total === 0
}
getListFn()
```
## 与axios对比
### 共同点
- 都支持Promise API
- 都同时支持Node和Browser环境
- 都支持请求／响应拦截器
- 都支持自动转换 JSON
### 不同点
- 浏览器环境： axios支持请求取消和全局配置，而 `fly 不支持请求取消` ，fly的配置支持实例级别和单次请求级别，其余功能基本不分伯仲，在体积上，fly.min.js只有4K左右，而axios.min.js 12K左右。Fly更轻量，集成成本更低。
- Node环境: Node下 Fly 的功能要明显强于axios，Fly在node下不仅提供了文件下载、上传的API，而且还可以通过 fly.$http 直接调用 request 库 的所有功能。
- 请求转发：Fly最大的特点就是`在混合APP中支持请求转发，而axios不支持`
- Http Engine: Fly中提出了Http Engine的概念，Fly可以通过更换Http Engine的方式实现很多有趣的功能，比如全局Ajax拦截
- 设计思想:Fly采用分层的设计思想，将上层用户接口和底层Http Engine分离。采用适配器模式，让实现Http Engine变的非常容易。正是这样的框架设计，可以通过替换底层Http Engine的方式，使得fly能够在灵活的支持各种环境的同时又能保证上层接口的一致性。还有，通过adapter，用户完全可以自定义http请求的实现
## 与Fetch的比较
- 必须手动设置header的 content-type，Fetch不会自动设置。
- 必须手动设置  credentials，Fetch默认不带cookie。
- 像40X、50X这种http 状态错误是不会触发catch，需要在then中处理。
- 不支持请求／响应拦截器，这在设置一些全局的参数、请求头时很有用。
- 不支持Node
- 浏览器支持程度不同。
