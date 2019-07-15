---
title: js-returnfun-async-more
date: 2019-03-06 17:48:07
author: T.2stt
comments: true
tags:
- js
- 回调/promise/async/awite
categories:
- [js]
---
>回调函数
# 回调函数
作为参数传递给另外一个函数，并且该回调函数在函数主体执行完后再执行

## 常见的回调函数以及存在的缺陷
**常见的回调函数：**
我们在 JQ 的 Ajax 请求中，可以看到回调的身影，经典的封装后的 Ajax 请求就是一个，Success后面所跟的函数就是一个回调函数。 
**存在的问题:**
因为JS是同步的，通过回调函数异步操作，会造成回调函数的嵌套，很影响代码的可读性。

---
# promise处理回调嵌套
## 什么是promise
promise通常用于处理函数的异步调用，通过链式调用的方式，使得代码更加直观
```javascript
var myPromise=function(tag){
    return new Promise(function(resolve,reject){
        if(tag){
            resolve('默认参数success');
        }else{
            reject('默认参数filed');
        }
    })
}
```
调用
```javascript
myPromise(true).then(function(message){
  console.log(message)
})  //"输出默认参数success"
```
我们可以看到设置 Promise 只要 new Promise() 就可以，并且接受一个函数，该函数里面会执行 **resolve()** 方法，表示异步调用成功时执行， **reject()** 表示异步调用失败时候调用。
在链式调用时候，**then** 后面接的第一个函数为成功之时调用的函数—**resolve**,并且这里的默认参数等同于promise中resolve中的初始参数。
## then 和 catch
**then** :可以在promise中实现链式调用，在上文中已经介绍。补充，then里面的第二个函数，为异步调用失败之时执行,接上面的例子
```javascript
myPromise(false).then(null,function(err){
  console.log(err)
})
//这样会输出"默认参数filed"
```
catch：catch方法，相当于then(null，function（err）{console.log(err)}失败方法调用的一个缩写。
```javascript
myPromise(false).catch(function(err){console.log(err)})
//这句话与上面等价，也会输出"默认参数filed"
```
## Promise.all
Promise.all 可以接收一个元素为 Promise 对象的数组作为参数，当这个数组里面所有的 Promise 对象都变为 resolve 时，该方法才会返回。
```javascript
var p1 = new Promise(function (resolve) {
    setTimeout(function () {
        resolve("Hello");
    }, 3000);
});

var p2 = new Promise(function (resolve) {
    setTimeout(function () {
        resolve("World");
    }, 1000);
});

Promise.all([p1, p2]).then(function (result) {
    console.log(result); // ["Hello", "World"]
});
```
# 为什么Async/Await比promise更好
## 简洁
使用Promise是这样的: 
getJSON函数返回一个promise，这个promise成功resolve时会返回一个json对象。我们只是调用这个函数，打印返回的JSON对象，然后返回”done”。
```javascript
const makeRequest = () =>
  getJSON()
    .then(data => {
      console.log(data)
      return "done"
    })

makeRequest()
```
使用Async/Await是这样的:
```javascript
const makeRequest = async () => {
  console.log(await getJSON())
  return "done"
}

makeRequest()
```
函数前面多了一个aync关键字。await关键字只能用在aync定义的函数内。async函数会隐式地返回一个promise，该promise的reosolve值就是函数return的值。(示例中reosolve值就是字符串”done”)。await getJSON()表示console.log会等到getJSON的promise成功reosolve之后再执行。

## 错误处理
Async/Await让try/catch可以同时处理同步和异步错误。在下面的promise示例中，try/catch不能处理JSON.parse的错误，因为它在Promise中。我们需要使用.catch，这样错误处理代码非常冗余。并且，在我们的实际生产代码会更加复杂。
```javascript
const makeRequest = () => {
  try {
    getJSON()
      .then(result => {
        // JSON.parse可能会出错
        const data = JSON.parse(result)
        console.log(data)
      })
      // 取消注释，处理异步代码的错误
      // .catch((err) => {
      //   console.log(err)
      // })
  } catch (err) {
    console.log(err)
  }
}
```
使用aync/await的话，catch能处理JSON.parse错误:
```javascript
const makeRequest = async () => {
  try {
    // this parse may fail
    const data = JSON.parse(await getJSON())
    console.log(data)
  } catch (err) {
    console.log(err)
  }
}
```
## 条件语句
下面实例中，通过判断返回数据来决定是直接返回，还是继续获取更多的数据。
```javascript
const makeRequest = () => {
  return getJSON()
    .then(data => {
      if (data.needsAnotherRequest) {
        return makeAnotherRequest(data)
          .then(moreData => {
            console.log(moreData)
            return moreData
          })
      } else {
        console.log(data)
        return data
      }
    })
}
```
这些代码看着就头痛。嵌套（6层），括号，return语句很容易让人感到迷茫，而它们只是需要将最终结果传递到最外层的Promise。
上面的代码使用async/await编写可以大大地提高可读性:
```javascript
const makeRequest = async () => {
  const data = await getJSON()
  if (data.needsAnotherRequest) {
    const moreData = await makeAnotherRequest(data);
    console.log(moreData)
    return moreData
  } else {
    console.log(data)
    return data    
  }
}
```
## 中间值
你很可能遇到过这样的场景，调用promise1，使用promise1返回的结果去调用promise2，然后使用两者的结果去调用promise3。你的代码很可能是这样的:
```javascript
const makeRequest = () => {
  return promise1()
    .then(value1 => {
      return promise2(value1)
        .then(value2 => {        
          return promise3(value1, value2)
        })
    })
}
```
如果promise3不需要value1，可以很简单地将promise嵌套铺平。如果你忍受不了嵌套，你可以将value 1 & 2 放进Promise.all来避免深层嵌套：
```javascript
const makeRequest = () => {
  return promise1()
    .then(value1 => {
      return Promise.all([value1, promise2(value1)])
    })
    .then(([value1, value2]) => {      
      return promise3(value1, value2)
    })
}
```
这种方法为了可读性牺牲了语义。除了避免嵌套，并没有其他理由将value1和value2放在一个数组中。
使用async/await的话，代码会变得异常简单和直观。
```javscript
const makeRequest = async () => {
  const value1 = await promise1()
  const value2 = await promise2(value1)
  return promise3(value1, value2)
}
```
## 错误栈
下面示例中调用了多个Promise，假设Promise链中某个地方抛出了一个错误:
```javascript
const makeRequest = () => {
  return callAPromise()
    .then(() => callAPromise())
    .then(() => callAPromise())
    .then(() => callAPromise())
    .then(() => callAPromise())
    .then(() => {
      throw new Error("oops");
    })
}

makeRequest()
  .catch(err => {
    console.log(err);
    // output
    // Error: oops at callAPromise.then.then.then.then.then (index.js:8:13)
  })
```
Promise链中返回的错误栈没有给出错误发生位置的线索。更糟糕的是，它会误导我们；错误栈中唯一的函数名为callAPromise，然而它和错误没有关系。(文件名和行号还是有用的)。

然而，async/await中的错误栈会指向错误所在的函数:
```javascript
const makeRequest = async () => {
  await callAPromise()
  await callAPromise()
  await callAPromise()
  await callAPromise()
  await callAPromise()
  throw new Error("oops");
}

makeRequest()
  .catch(err => {
    console.log(err);
    // output
    // Error: oops at makeRequest (index.js:7:9)
  })
```
在开发环境中，这一点优势并不大。但是，当你分析生产环境的错误日志时，它将非常有用。这时，知道错误发生在makeRequest比知道错误发生在then链中要好。




