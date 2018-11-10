---
title: -常见功能实现
date: 2018-11-09 17:50:27
author: T.2stt
comments: true
# password: qwertyuiop
tags:
- [wx小程序]
categories:
- [wx小程序]
---
>摇一摇、页面内发起转发

# 摇一摇
## [加速器](https://developers.weixin.qq.com/miniprogram/dev/api/device/accelerometer/wx.onAccelerometerChange.html)
## shakeFn
```bash
shakeFn: function () {
    var determination = false
    var that = this
    function a(){
        wx.onAccelerometerChange(function(res) {
            var curTime = new Date().getTime()
            var SHAKE_THRESHOLD = 60 // 间隔时间
            var last_update = that.data.last_update
            var len = util.res.length
            var list = Math.floor(Math.random()*(len-1))
            if ((curTime - last_update) > 100) {
                var diffTime = curTime - last_update; 
                var speed = Math.abs(res.x + res.y + res.z - that.data.last_x - that.data.last_y - that.data.last_z) / diffTime * 10000;
                if (speed > SHAKE_THRESHOLD && !determination) {
                    determination = true
                    determination = that.shakeOverFn()
                    // determination = that.f(util.res[list])
                }
                that.setData({
                    last_update: curTime,
                    last_x: res.x,
                    last_y: res.y,
                    last_z: res.z
                })
            }
        })
    }
    a()
},
```
## shakeOverFn
```bash
shakeOverFn: function(res){
    // 请求接口
    wx.stopAccelerometer(); // 关闭检测
    this.setData({
        isShowResultPrize: false
    });
    wx.playBackgroundAudio({
        dataUrl: 'http://fjyd.sc.chinaz.com/files/download/sound1/201410/5012.mp3',
        title: 'weixin'
    })
    return false
},
```

---
# 页面内发起转发
```bash
<button class="text_" open-type="share">炫耀一下</button>
1. open-type="share"必须；
2. 只有button才可以实现；
```