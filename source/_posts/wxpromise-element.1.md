---
title: wxpromise的元素（试图容器）
date: 2018-05-07 20:26:31
author: T.2stt
comments: true
tags:
- wxprogram-element
categories:
- wxprogram
---

>此章节记载的是 wxpromise的元素与html的差别,通过对比的方式进行学习
navigator-进行界面的跳转、小程序的跳转、tab的跳转

# 基本元素

## view

[官网](https://developers.weixin.qq.com/miniprogram/dev/component/view.html)

| 属性名        | 类型    |默认值        | 说明    |
| --------   | :-----:   | :-----:   | :-----:   |
| hover-class   | String      |无   | 指定按下去的样式类。当  hover-class="none"  时，没有点击态效果      |
| hover-stop-propagation   | Boolean |false   | 指定是否阻止本节点的祖先节点出现点击态    |
| hover-start-time   | Number      |50  | 按住后多久出现点击态 （单位毫秒）    |
| hover-stay-tim   | Number      |400  | 手指松开后点击态保留时间      |
###  项目总结
```bash
	1.属于块级元素（div）;
	2.如果需要使用滚动视图，请使用 scroll-view;
```

---

## scroll-view
可滚动视图区域。
[官网](https://developers.weixin.qq.com/miniprogram/dev/component/scroll-view.html)

| 属性名        | 类型    |默认值        | 说明    |
| --------   | :-----:   | :-----:   | :-----:   |
| scroll-x   | Boolean |false| 允许横向滚动 |
| scroll-y | Boolean |false   | 允许纵向滚动    |
| upper-threshold   | Number |50  | 距顶部/左边多远时（单位px），触发 scrolltoupper 事件	 |
| lower-threshold  | Number |50  | 距底部/右边多远时（单位px），触发 scrolltolower 事件	 |
| scroll-top | Number |  | 设置竖向滚动条位置|
| scroll-left | Number |  | 设置横向滚动条位置|
| scroll-into-view | String |  | 设值应为某子元素id（id不能以数字开头）。设置哪个方向可滚动，则在哪个方向滚动到该元素置横向滚动条位置|
| scroll-with-animation | Boolean | false | 在设置滚动条位置时使用动画过渡|
| enable-back-to-top| String | false | iOS点击顶部状态栏、安卓双击标题栏时，滚动条返回顶部，只支持竖向|
| bindscrolltolower | EventHandle |  | 滚动到底部/右边，会触发 scrolltolower 事件|
| bindscroll | EventHandle || 滚动时触发，event.detail = {scrollLeft, scrollTop, scrollHeight, scrollWidth, deltaX, deltaY}|


<font color="red">【注意：】</font>

1. 使用竖向滚动时，需要给<scroll-view/>一个固定高度，通过 WXSS 设置 height。
2. 请勿在 scroll-view 中使用 textarea、map、canvas、video 组件
3. scroll-into-view 的优先级高于 scroll-top
4. 在滚动 scroll-view 时会阻止页面回弹，所以在 scroll-view 中滚动，是无法触发 onPullDownRefresh
5. 若要使用下拉刷新，请使用页面的滚动，而不是 scroll-view ，这样也能通过点击顶部状态栏回到页面顶部
---

## swiper
滑块视图容器。
[官网](https://developers.weixin.qq.com/miniprogram/dev/component/swiper.html)

| 属性名        | 类型    |默认值        | 说明    |
| --------   | :-----:   | :-----:   | :-----:   |
| indicator-dots | Boolean |false| 是否显示面板指示点 |
| indicator-color | Color |rgba(0, 0, 0, .3)| 指示点颜色 |
| indicator-active-color| Color |#000000  | 当前选中的指示点颜色|
| autoplay| Boolean |false| 是否自动切换|
| current| Number | 0 | 当前所在滑块的 index|
| current-item-id| String | "" | 当前所在滑块的 item-id ，不能与 current 被同时指定|
| interval | Number | 5000 | 自动切换时间间隔|
| duration | Number | 5000 | 滑动动画时长|
| circular| Boolean | false | 是否采用衔接滑动|
| vertical | Boolean | false | 滑动方向是否为纵向|
| previous-margin | String |"0px"| 后边距，可用于露出后一项的一小部分，接受 px 和 rpx 值|
| display-multiple-items | Number |1| 同时显示的滑块数量|
| skip-hidden-item-layout| Boolean | false | 是否跳过未显示的滑块布局，设为 true 可优化复杂情况下的滑动性能，但会丢失隐藏状态滑块的布局信息|
| bindchange | EventHandle |-| current 改变时会触发 change 事件，event.detail = {current: current, source: source}|
| bindanimationfinish | EventHandle |-|动画结束时会触发 animationfinish 事件，event.detail 同上|

change事件返回detail中包含一个source字段，表示导致变更的原因，可能值如下：
- autoplay 自动播放导致swiper变化；
- touch 用户划动引起swiper变化；
- 其他原因将用空字符串表示。

### swiper-item
仅可放置在<swiper/>组件中，宽高自动设置为100%。


| 属性名        | 类型    |默认值        | 说明    |
| --------   | :-----:   | :-----:   | :-----:   |
| item-id | String |""| 该 swiper-item 的标识符 |

<font color="red">【注意：】</font>

1.  如果在 bindchange 的事件回调函数中使用 setData 改变 current 值，则有可能导致 setData 被不停地调用，因而通常情况下请在改变 current 值前检测 source 字段来判断是否是由于用户触摸引起。
---
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

# 基础内容

## text
| 属性名        | 类型    |默认值        | 说明    |
| --------   | :-----:   | :-----:   | :-----:   |
| selectable   | selectable |false| 文本是否可选 |
| space | String |false   | 显示连续空格    |
| decode   | Boolean |false  | 是否解码	 |

space 有效值：

| 值        | 说明    |
| --------   | :-----:   | 
| ensp   | 中文字符空格一半大小 |
| emsp | 中文字符空格大小 |
| nbsp   | 根据字体设置的空格大小 |

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

# 媒体组建

## image
| 属性名        | 类型    |默认值        | 说明    |
| --------   | :-----:   | :-----:   | :-----:   |
|src|	String|	|图片资源地址，支持云文件ID（2.2.3起）	|
|mode|	String|	'scaleToFill'	|图片裁剪、缩放的模式|	
|lazy-load|	Boolean|	false|	图片懒加载。只针对page与scroll-view下的image有效|
|binderror|	HandleEvent| |当错误发生时，发布到 AppService 的事件名，事件对象event.detail = {errMsg: 'something wrong'}|	
|bindload|	HandleEvent||当图片载入完毕时，发布到 AppService 的事件名，事件对象event.detail = {height:'图片高度px', width:'图片宽度px'}|

<font color="red">【注意：】</font>

```bash
1.image组件默认宽度300px、高度225px;
2.image组件中二维码/小程序码图片不支持长按识别。仅在wx.previewImage中支持长按识别。
```


### mode 有效值：
mode 有 13 种模式，其中 4 种是缩放模式，9 种是裁剪模式。<font color="red">裁剪是按照位置裁剪</font>

| 模式        | 值    |说明|
| --------   | :-----:   | :-----:   |
|缩放|	scaleToFill	|不保持纵横比缩放图片，使图片的宽高完全拉伸至填满 image 元素|
|缩放|	aspectFit	|保持纵横比缩放图片，使图片的长边能完全显示出来。也就是说，可以完整地将图片显示出来。|
|缩放|	aspectFill	|保持纵横比缩放图片，只保证图片的短边能完全显示出来。也就是说，图片通常只在水平或垂直方向是完整的，另一个方向将会发生截取。|
|裁剪|	top/bottom/center/left/right/top left/top right	/bottom left/bottom right	|不缩放图片，只显示图片的顶部/底部/中间/左边/右边/左上/右上/左下/右下边区域|

<font color="red">原图：</font>
![原图](https://developers.weixin.qq.com/miniprogram/dev/image/cat/0.jpg?t=18101520)

<font color="red">scaleToFill</font>
不保持纵横比缩放图片，使图片完全适应
![原图](https://developers.weixin.qq.com/miniprogram/dev/image/cat/1.png?t=18101520)

<font color="red">aspectFit</font>
保持纵横比缩放图片，使图片的长边能完全显示出来
![原图](https://developers.weixin.qq.com/miniprogram/dev/image/cat/2.png?t=18101520)

<font color="red">aspectFill</font>
保持纵横比缩放图片，使图片的长边能完全显示出来
![原图](https://developers.weixin.qq.com/miniprogram/dev/image/cat/3.png?t=18101520)

<font color="red">top</font>
不缩放图片，只显示图片的顶部区域
![原图](https://developers.weixin.qq.com/miniprogram/dev/image/cat/4.png?t=18101520)

<font color="red">bottom</font>
不缩放图片，只显示图片的底部区域
![原图](https://developers.weixin.qq.com/miniprogram/dev/image/cat/5.png?t=18101520)

<font color="red">center</font>
不缩放图片，只显示图片的中间区域
![原图](https://developers.weixin.qq.com/miniprogram/dev/image/cat/6.png?t=18101520)

<font color="red">left</font>
不缩放图片，只显示图片的左边区域
![原图](https://developers.weixin.qq.com/miniprogram/dev/image/cat/7.png?t=18101520)

<font color="red">right</font>
不缩放图片，只显示图片的右边边区域
![原图](https://developers.weixin.qq.com/miniprogram/dev/image/cat/8.png?t=18101520)

<font color="red">top left</font>
不缩放图片，只显示图片的左上边区域
![原图](https://developers.weixin.qq.com/miniprogram/dev/image/cat/9.png?t=18101520)

<font color="red">top right</font>
不缩放图片，只显示图片的右上边区域
![原图](https://developers.weixin.qq.com/miniprogram/dev/image/cat/10.png?t=18101520)

<font color="red">bottom left</font>
不缩放图片，只显示图片的左下边区域
![原图](https://developers.weixin.qq.com/miniprogram/dev/image/cat/11.png?t=18101520)

<font color="red">bottom right</font>
不缩放图片，只显示图片的右下边区域
![原图](https://developers.weixin.qq.com/miniprogram/dev/image/cat/12.png?t=18101520)

---

# 导航
## navigator
| 属性名        | 类型    |默认值        | 说明    |
| --------   | :-----:   | :-----:   | :-----:   |
|target| String|self|在哪个目标上发生跳转，默认当前小程序，可选值self/miniProgram|
|url|	String|	|当前小程序内的跳转链接|	
|open-type|	String|	navigate|跳转方式|
|delta|	Number| |当 open-type 为 'navigateBack' 时有效，表示回退的层数|	
|app-id	|String||当target="miniProgram"时有效，要打开的小程序 appId|
|path| String||当target="miniProgram"时有效，打开的页面路径，如果为空则打开首页|
|extra-data|Object|	|当target="miniProgram"时有效，需要传递给目标小程序的数据，目标小程序可在 App.onLaunch()，App.onShow() 中获取到这份数据。|	
|version|version|release|当target="miniProgram"时有效，要打开的小程序版本，有效值 develop（开发版），trial（体验版），release（正式版），仅在当前小程序为开发版或体验版时此参数有效；如果当前小程序是正式版，则打开的小程序必定是正式版。|
|hover-class|String| navigator-hover|指定点击时的样式类，当hover-class="none"时，没有点击态效果|	
|hover-stop-propagation|Boolean|false|指定是否阻止本节点的祖先节点出现点击态|
|hover-start-time|Number|50|按住后多久出现点击态，单位毫秒|
|hover-stay-time|Number|600|手指松开后点击态保留时间，单位毫秒|
|bindsuccess|String||当target="miniProgram"时有效，跳转小程序成功|
|bindfail|String||	当target="miniProgram"时有效，跳转小程序失败|
|bindcomplete|String||当target="miniProgram"时有效，跳转小程序完成|

### open-type 有效值：
| 值        | 说明    |
| --------   | :-----:   |
|navigate| 对应 wx.navigateTo 或 wx.navigateToMiniProgram 的功能|
|redirect|	对应 wx.redirectTo 的功能|	
|switchTab|	对应 wx.switchTab 的功能|
|reLaunch|	对应 wx.reLaunch 的功能|	
|navigateBack	|对应 wx.navigateBack 的功能|
|exit| 退出小程序，target="miniProgram"时生效|

<font color="red">【注意：】</font>
1. navigator-hover 默认为 {background-color: rgba(0, 0, 0, 0.1); opacity: 0.7;}, <navigator/> 的子节点背景色应为透明色