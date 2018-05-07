---
title: wxpromise的图片
date: 2018-05-06 13:54:43
author: T.2stt
comments: true
tags:
- wxprogram-image
categories:
- wxprogram
---
>此章节记载的是微信小程序关于图片的一些功能。
wx.chooseImage(OBJECT),从本地相册选择图片或使用相机拍照。
wx.previewImage(OBJECT),图片的预览。
wx.getImageInfo(OBJECT),获取图片信息。
wx.saveImageToPhotosAlbum(OBJECT),保存图片到系统相册。需要用户授权 scope.writePhotosAlbum

## wx.chooseImage
	chooseImage只是选择图片，限制图片选择张数+图片选择是否压缩等名获取到文件信息（路劲，大小），如果需要上传则选择别的API。
	wx.previewImage(OBJECT),预览图片。

### 示例代码
``` bash
	wx.chooseImage({
	  count: 1, // 默认9
	  sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
	  sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有
	  success: function (res) {
	    // 返回选定照片的本地文件路径列表，tempFilePath可以作为img标签的src属性显示图片
	    var tempFilePaths = res.tempFilePaths
	  }
	})
```

### OBJECT参数说明
| 参数        | 类型    |  必填  | 说明  |
| --------   | :-----:   | :----: | :----: |
| <font face="fantasy">count</font>      | Number      |   否    |   最多可以选择的图片张数，默认9    |
| <font face="fantasy">sizeType</font>   | StringArray |   否    |   original 原图，compressed 压缩图，默认二者都有    |
| <font face="fantasy">sourceType</font> | StringArray |   否    |   album 从相册选图，camera 使用相机，默认二者都有    |
| <font face="fantasy">success</font>    | Function    |   是    |   成功则返回图片的本地文件路径列表 tempFilePaths    |
| <font face="fantasy">fail</font>       | Function    |   否    |   接口调用失败的回调函数    |
| <font face="fantasy">complete</font>   | Function    |   否    |   接口调用结束的回调函数（调用成功、失败都会执行）    |


## wx.previewImage
	预览图片。

### 示例代码
```bash
	wx.previewImage({
	  current: '', // 当前显示图片的http链接
	  urls: [] // 需要预览的图片http链接列表
	})
```
### OBJECT参数说明
| 参数        | 类型    |  必填  | 说明  |
| --------   | :-----:   | :----: | :----: |
| <font face="fantasy">current</font>      | String      |   否    |   当前显示图片的链接，不填则默认为 urls 的第一张    |
| <font face="fantasy">urls</font>   | StringArray |   是    |   需要预览的图片链接列表    |
| <font face="fantasy">success</font>    | Function    |   是    |   接口调用成功的回调函数    |
| <font face="fantasy">fail</font>       | Function    |   否    |   接口调用失败的回调函数    |
| <font face="fantasy">complete</font>   | Function    |   否    |   接口调用结束的回调函数（调用成功、失败都会执行）    |

## wx.getImageInfo
	获取图片信息。

### 示例代码
```bash
	wx.getImageInfo({
	  src: 'images/a.jpg',
	  success: function (res) {
	    console.log(res.width)
	    console.log(res.height)
	  }
	})
	
	wx.chooseImage({
	  success: function (res) {
	    wx.getImageInfo({
	      src: res.tempFilePaths[0],
	      success: function (res) {
	        console.log(res.width)
	        console.log(res.height)
	      }
	    })
	  }
	})
```
### OBJECT参数说明
| 参数        | 类型    |  必填  | 说明  |
| --------   | :-----:   | :----: | :----: |
| <font face="fantasy">src</font>      | String      |   否    |   图片的路径，可以是相对路径，临时文件路径，存储文件路径，网络图片路径    |
| <font face="fantasy">success</font>    | Function    |   是    |   接口调用成功的回调函数    |
| <font face="fantasy">fail</font>       | Function    |   否    |   接口调用失败的回调函数    |
| <font face="fantasy">complete</font>   | Function    |   否    |   接口调用结束的回调函数（调用成功、失败都会执行）    |

#### 解释
``` bash
1.success返回参数说明：
  1.1 width----StringArra类型，图片的本地文件路径列表
  1.2 height----ObjectArray类型，图片的本地文件列表，每一项是一个 File 对象
4.File 对象结构如下:
  4.1 path----Number类型，图片宽度，单位px
  4.2 size----Number类型，图片宽度，单位px
  4.3 path----String类型，返回图片的本地路径
  4.4 orientation----String类型，返回图片的方向，有效值见下表
  4.5 type----String类型，返回图片的格式
```
#### orientation参数说明：
| 参数        | 枚举值    |
| --------   | :-----:   |
| <font face="fantasy">up</font>            | 默认      |
| <font face="fantasy">down</font>          | 180度旋转 |
| <font face="fantasy">left</font>          | 逆时针旋转90度 |
| <font face="fantasy">right</font>         | 顺时针旋转90度    |
| <font face="fantasy">up-mirrored</font>   | 同up，但水平翻转    |
| <font face="fantasy">down-mirrored</font> | 同down，但水平翻转    |
| <font face="fantasy">left-mirrored</font> | 同left，但垂直翻转    |
| <font face="fantasy">right-mirrored</font>| 同right，但垂直翻转    |

## wx.saveImageToPhotosAlbum
	保存图片到系统相册。需要用户授权 scope.writePhotosAlbum。

### 示例代码
```bash
	wx.saveImageToPhotosAlbum({
	    success(res) {
	    }
	})
```
### OBJECT参数说明
| 参数        | 类型    |  必填  | 说明  |
| --------   | :-----:   | :----: | :----: |
| <font face="fantasy">filePath</font>      | String      |   是    |   图片文件路径，可以是临时文件路径也可以是永久文件路径，不支持网络图片路径    |
| <font face="fantasy">success</font>    | Function    |   否    |   接口调用成功的回调函数    |
| <font face="fantasy">fail</font>       | Function    |   否    |   接口调用失败的回调函数    |
| <font face="fantasy">complete</font>   | Function    |   否    |   接口调用结束的回调函数（调用成功、失败都会执行）    |

#### 解释
```bash
	1.success返回参数说明：
		errMsg----String类型，调用结果
```