---
title: -框架搭建
date: 2018-11-09 17:53:04
author: T.2stt
comments: true
tags:
- [wx小程序]
categories:
- [wx小程序]
---
>记载一般项目的搭建的操作，require请求的封装，wxss的公共样式，iconfont的引入

# require请求的封装
## http.js
```bash
let header = {
    'Accept': 'application/json',
    'content-type': 'application/json',
    'mm_mall_token': ''
}
class request {
    constructor() {
        this._baseUrl = 'https://mall.xchean.com/api'
    }
    /**
    * 设置统一的异常处理
    */
    setErrorHandler(handler) {
        this._errorHandler = handler;
    }

    /**
    * GET类型的网络请求
    */
    getRequest(httpObj) {
        httpObj.header = header
        httpObj.method = 'GET'
        return this.requestAll(httpObj)
    }

    /**
    * DELETE类型的网络请求
    */
    deleteRequest(httpObj) {
        httpObj.header = header
        httpObj.method = 'DELETE'
        return this.requestAll(httpObj)
    }

    /**
    * PUT类型的网络请求
    */
    putRequest(httpObj) {
        httpObj.header = header
        httpObj.method = 'PUT'
        return this.requestAll(httpObj)
    }

    /**
    * POST类型的网络请求
    */
    postRequest(httpObj) {
        httpObj.header = header
        httpObj.method = 'POST'
        return this.requestAll(httpObj)
    }

    /**
    * 网络请求
    */
    requestAll(httpObj) {
        return new Promise((resolve, reject) => {
            wx.showLoading({
                title: '加载中' //数据请求前loading
            })
            wx.request({
                url: this._baseUrl + httpObj.url,
                data: httpObj.data,
                header: httpObj.header,
                method: httpObj.method,
                success: (res => {
                    wx.hideLoading();
                    if (res.statusCode === 200) {
                        //200: 服务端业务处理正常结束
                        resolve(res)
                    } else {
                        //其它错误，提示用户错误信息
                        if (this._errorHandler != null) {
                        //如果有统一的异常处理，就先调用统一异常处理函数对异常进行处理
                        this._errorHandler(res)
                        }
                        reject(res)
                    }
                }),
                fail: (res => {
                    wx.hideLoading();
                    if (this._errorHandler != null) {
                        this._errorHandler(res)
                    }
                    reject(res)
                })
            })
        })
    }
}

module.exports = {
  request, header
} 
```
将header暴露出去，在登录后进行更改，app.js中进行更改
## servise.js
```bash
import http from './http.js'
import url_ from '/allApi.js'
class servise {
    constructor() {
        this.api = url_.URL
        this._request = new http.request()
        this._request.setErrorHandler(this.errorHander)
    }

    /**
    * 统一的异常处理方法
    */
    errorHander(res) {
        console.error(res)
    }

    /**
    * 查询所有新闻列表
    */
    moduleListSer(postData) {
        return this._request.getRequest({
            url: this.api.moduleList,
            data: postData,
        }).then(res => res.data)
    }
}
export default servise
```

## app.js
```bash
var http = require('./utils/apis/http.js')
import servise from './utils/apis/servise.js'
App({
    servise:new servise(),
    onLaunch: function () {
        // 测试更改token
        let this_ = this
        let fn = function(){
            let postData = {typ: '1,2,3,4'}
            this_.servise.moduleListSer(postData)
            http.header.mm_mall_token = '8787878787';//给header 赋值
        }
        fn()
    },
    globalData: {
        userInfo: null
    }
})
```
## 使用方法
```bash
let postData = {typ: '1,2,3,4'}
app.servise.moduleListSer(postData)
```
# wxss的公共样式(初始化)
```bash
@import "utils/wxss/public.wxss";
@import "utils/wxss/iconfont.wxss";
page{
  height: 100%;
  background: #F4F4F4;
}
.container{
  font-size: 32rpx;
  border-top: 1rpx solid #f4f4f4;
  height: 100%;
  overflow: auto;
}
view,text,textarea{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  line-height: 1;
}
image{
  vertical-align: middle;
}
::-webkit-scrollbar {
  width: 0;
  height: 0;
  color: transparent;
}
button {
  font-size: 28rpx;
  background-color: transparent;
  border: none;
  padding: 0;
  margin: 0;
  line-height: 1;
}
button::after {
  border: none;
}
```
# iconfont的引入
## 新建iconfont.wxss；
## app.wxss引入；
## 复制unicode的代码计入iconfont.wxss；
```bash
@font-face {
  font-family: 'iconfont';  /* project id 901403 */
  src: url('//at.alicdn.com/t/font_901403_wpizt398wi.eot');
  src: url('//at.alicdn.com/t/font_901403_wpizt398wi.eot?#iefix') format('embedded-opentype'),
  url('//at.alicdn.com/t/font_901403_wpizt398wi.woff') format('woff'),
  url('//at.alicdn.com/t/font_901403_wpizt398wi.ttf') format('truetype'),
  url('//at.alicdn.com/t/font_901403_wpizt398wi.svg#iconfont') format('svg');
}
```
## 更改文字大小
```bash
.iconfont {
  font-family:"iconfont" !important;
  font-size:32rpx;
  font-style:normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
```
## 下载代码，将icon-xxx:before...复制
```bash
.icon-fenxiang:before { content: "\e667"; }

.icon-zuoshuangyinhao:before { content: "\e62e"; }

.icon-dayuhao:before { content: "\e634"; }
```
## 使用
```bash
<text class="iconfont icon-fenxiang" />
```