---
title: react-redux
date: 2019-07-21 20:25:17
tags:
---

## 中间件
applyMiddleware();

## combinedReducer

## store.getState()

## 请求回来之后去更改数据
thunkMiddleware 中间件可以dispath一个function
success()=>{
    return {
        type: 'CHSN_A',
        des: 'sdsd'
    }
}
dispath(loading());
dispath(success());
返回一个dispath给你

## 源码
action调用dispath后调用reducer后会监听数据（循环）即store.subscribe() // 界面刷新
reducer最好都要判断action的type 否则返回return 原值 因为redux会默认给你一些值，所以只有自己要改的才改

## react-redux
### 开始
```
<Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')

```