---
title: -元素（试图容器）
date: 2018-05-07 20:26:31
author: T.2stt
comments: true
tags:
- [wx小程序，image元素]
categories:
- [wx小程序]
---

>此章节记载的是 wxpromise的元素与html的差别,通过对比的方式进行学习
navigator-进行界面的跳转、小程序的跳转、tab的跳转

# 基本元素

## view

[官网](https://developers.weixin.qq.com/miniprogram/dev/component/view.html)
###  项目总结
```bash
	1.属于块级元素（div）;
	2.如果需要使用滚动视图，请使用 scroll-view;
```

---

## scroll-view
可滚动视图区域。
[官网](https://developers.weixin.qq.com/miniprogram/dev/component/scroll-view.html)

<font color="red">【注意：】</font>

1. 使用竖向滚动时，需要给<scroll-view/>一个固定高度，通过 WXSS 设置 height。
2. 请勿在 scroll-view 中使用 textarea、map、canvas、video 组件
3. scroll-into-view 的优先级高于 scroll-top
4. 在滚动 scroll-view 时会阻止页面回弹，所以在 scroll-view 中滚动，是无法触发 onPullDownRefresh
5. 若要使用下拉刷新，请使用页面的滚动，而不是 scroll-view ，这样也能通过点击顶部状态栏回到页面顶部
6. 如何实现相应的滚动（点击分类，滚动商品到对应的分类处）
```bash
<scroll-view class="list_box" scroll-y="true" scroll-into-view="currentType_{{selected}}" bindscroll="goodsScrollFn" bindtouchstart="touchstartFn">
    <view style="position: relative;" id="currentType_{{index}}" wx:for="{{menu}}" wx:key="" wx:for-item="info">
        <view class="title_">{{info.typeName}}</view>
        <view class="info_box">
            <block wx:for="{{info.menuContent}}" wx:key="">
                商品的内容块
            </block>
        </view>
    </view>
</scroll-view>
```
    <font color="red">【解析其实现原理以及注意事项】</font>
    1. 商品信息分类以及商品要全部给出，不要用v-if进行部分展示；
    2. scroll-into-view="currentType_{{selected}}"要与要被滚动到的商品的id="currentType_{{index}}"一致对应上；
    3. 设置滚动事件
    ```bash
    goodsScrollFn: function (e){
        let _top = e.detail.scrollTop;
        let progress = parseInt(_top / this.data.ht); // 计算出 当前的下标
        if (progress > this.data.selected) { // 向上拉动屏幕
            this.setData({ selected: progress });
            this.turnMenu(this.data.selected);
        } else if (progress == this.data.selected) {
        return false;
        } else { // 向下拉动屏幕
            if (this.data.isTouch) {
                this.setData({
                    selected: progress == 0 ? 0 : progress--
                });
                this.turnMenu(progress);
            }
        }
    },
    ```
    其中ht是在onload的时候获得
    ```bash
    onLoad: function (options) {
        // 框架尺寸设置
        wx.getSystemInfo({
            success: (options) => {
            var wd = options.screenWidth; // 页面宽度
            var ht = options.windowHeight; // 页面高度
            this.setData({ wd: wd, ht: ht })
            }
        });
    },
    ```
    会发现问题，当点击分类的时候也会出发滚动事件（progress为0的时候）
    所以需要去阻止：bindtouchstart="touchstartFn"   
    ```bash
<!-- 点击的时候标志开始滚动 -->
touchstartFn: function(e) {
    this.setData({
        isTouch:true
    })
},
在isTouch的时候才让其执行滚动事件，否则执行turnMenu事件，其中turnMenu事件中
turnMenu: function (e) {
    let index_= '';
    if (e.currentTarget){// 点击的时候
        this.setData({
            isTouch:false // 非touch事件
        })
        index_ = e.currentTarget.dataset.index
    } else {
        index_ = e
    }
    this.setData({
        selected: index_
    })
},
```
---

## swiper
滑块视图容器。
[官网](https://developers.weixin.qq.com/miniprogram/dev/component/swiper.html)

<font color="red">【注意：】</font>

1.  如果在 bindchange 的事件回调函数中使用 setData 改变 current 值，则有可能导致 setData 被不停地调用，因而通常情况下请在改变 current 值前检测 source 字段来判断是否是由于用户触摸引起。
2. swiper的小点点的样式更改通过.wx-swiper-dots进行更改
```bash
<swiper class="swiper-box" indicator-dots="{{ indicatordots }}" autoplay="{{ autoplay }}">
    <block wx:for="{{ swiperItem }}">
        <swiper-item>
            <navigator url="{{ item.linkUrl }}">
                <image class="slide-image" src="{{ item.imgUrl }}"></image>
            </navigator>
        </swiper-item>
    </block>
</swiper>
.swiper-box .wx-swiper-dots.wx-swiper-dots-horizontal{
     margin-bottom: 2rpx;
}
.swiper-box .wx-swiper-dot{
    width:40rpx;
    display: inline-flex;
    height: 10rpx;
    margin-left: 20rpx;
    justify-content:space-between;
}
.swiper-box .wx-swiper-dot::before{
    content: '';
    flex-grow: 1; 
    background: rgba(255,255,255,0.8);
    border-radius: 8rpx
}
.swiper-box .wx-swiper-dot-active::before{
    background:rgba(244,0,0,0.8);   
}
```
---

# 基础内容

## text
[官网](https://developers.weixin.qq.com/miniprogram/dev/component/text.html)

<font color="red">Tips</font>

```bash
1.decode可以解析的有 &nbsp; &lt; &gt; &amp; &apos; &ensp; &emsp;
2.各个操作系统的空格标准并不一致。
3.<text/> 组件内只支持 <text/> 嵌套。
4.除了文本节点以外的其他节点都无法长按选中。
```

### 项目总结
```bash
	1.属于行内元素（span）;
	2.低版本的<text/> 组件内嵌的 <text/> style 设置可能不会生效；
```
---


# 媒体组建

## image
[官网](https://developers.weixin.qq.com/miniprogram/dev/component/image.html)

<font color="red">【注意：】</font>
```bash
1.image组件默认宽度300px、高度225px;
2.image组件中二维码/小程序码图片不支持长按识别。仅在wx.previewImage中支持长按识别。
```
---

# 导航

## navigator
[官网](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html)

<font color="red">【注意：】</font>
1. navigator-hover 默认为 {background-color: rgba(0, 0, 0, 0.1); opacity: 0.7;}, <navigator/> 的子节点背景色应为透明色