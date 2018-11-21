---
title: -数据缓存storage
date: 2018-11-15 09:28:06
author: T.2stt
comments: true
tags:
- [wx小程序]
categories:
- [wx小程序]
---
>[相关官网链接](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.clearStorage.html)
存储的时间与localstorage一样
比较同步存储---try/catch、异步存储success后再操作（差异点）

# 存
## 异步
wx.setStorage(OBJECT)
wx.setStorage({key:'存入的键的名字',data:'存入的值（可以为对象，字符串，数组）'})
键与值是以对象的形势存入
```bash
wx.setStorage({
  key:"key",
  data:"value"
})

<!-- 存入结果 -->
key: value
```
## 同步
wx.setStorageSync(KEY,DATA)
wx.setStorageSync('存入的键的名字', '存入的值（可以为对象，字符串，数组）')
```bash
wx.setStorageSync('loginInfo',loginInfo)
<!-- 结果 -->
loginInfo: loginInfo
```
# 取
## 异步
wx.getStorage(OBJECT)
```bash
wx.getStorage({
  key: '需要取值的键名',
  success (res) {
    成功后的操作
  } 
})
```
## 同步
wx.getStorageSync(KEY)
```bash
try {
 var value = wx.getStorageSync('key')
 if (value) {
   // Do something with return value
 }
} catch (e) {
 // Do something when catch error
}
```
# 获取当前storage的相关信息
## 异步
wx.getStorageInfo(OBJECT)
```bash
wx.getStorageInfo({
  success (res) {
    console.log(res.keys)
    console.log(res.currentSize)
    console.log(res.limitSize)
  }
})
```
## 同步
wx.getStorageInfoSync()
```bash
try {
 var res = wx.getStorageInfoSync()
 console.log(res.keys)
 console.log(res.currentSize)
 console.log(res.limitSize)
} catch (e) {
 // Do something when catch error
}
```
# 删
## 异步
wx.removeStorage(OBJECT)
```bash
wx.removeStorage({
  key: '需要删除的键名',
  success (res) {
    console.log(res.data)
  } 
})
```
## 同步
wx.removeStorageSync(KEY)
```bash
try {
 wx.removeStorageSync('key')
} catch (e) {
 // Do something when catch error
}
```
# 清空
## 异步
wx.clearStorage()

## 同步
```bash
try {
  wx.clearStorageSync()
} catch(e) {
 // Do something when catch error
}
```
