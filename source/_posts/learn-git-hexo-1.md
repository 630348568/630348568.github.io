---
title: git-Hexo搭建填坑
date: 2018-2-12 10:50:01
author: T.stt
tags:
- git-hexo
categories:
- [工具,hexo]
---

> 记录hexo的开发操作,包括安装hexo--部署hexo--hexo命令--操作中的报错处理--hexo管理--hexo使用的插件。一步步的积累，一步步的改善，制作自己的一个小博客

## hexo 原理
``` bash
	hexo在执行hexo g时会在本地先把博客生成的一套静态站点放到public文件夹中，再执行hexo d时将其复制到.deploy文件夹中。
	Github的版本库通常建议同时附上README.md说明文件，但是hexo默认情况下会把所有md文件解析成html文件，所以即使你在线生成了README.md，
	它也会在你下一次部署时被删去。在执行hexo deploy前把在本地写好的README.md文件复制到.deploy文件夹中，再去执行hexo deploy。
```

## 安装Hexo
命令：`npm install -g hexo`
``` bash
	这里可能安装失败，可能权限不够，在命令前加sudo： sudo npm install -g hexo
```

## 部署Hexo
`hexo init`     // 这个命令会初始化博客的目录，所以，执行这个命令时，在你想创建的目录下执行，就自动生成到对应目录下。

``` bash
执行命令,会在当前命令的路径下，生成以下文件：
.
    ├── .deploy      //执行hexo deploy命令部署到GitHub上的内容目录
    ├── public       //执行hexo generate命令，输出的静态网页内容目录
    ├── scaffolds    //layout模板文件目录，其中的md文件可以添加编辑
    ├── scripts      //扩展脚本目录，这里可以自定义一些javascript脚本
    ├── source       //文章源码目录，该目录下的markdown和html文件均会被hexo处理。该页面对应repo的根目录，404文件、favicon.ico文件，CNAME文件等都应该放这里，该目录下可新建页面目录。
    |   ├── _drafts  //草稿文章
    |   └── _posts   //发布文章
    ├── themes       //主题文件目录
    ├── _config.yml  //全局配置文件，大多数的设置都在这里
    └── package.json //应用程序数据，指明hexo的版本等信息，类似于一般软件中的关于按钮
```
## Hexo命令
Hexo下，通过 _config.yml 设置博客，可以想象成我们用的软件里的设置一样，只是它通过一个文件列出这些参数，然后让我们填写和修改。

### 全局设置

``` bash
全局设置:_config.yml，打开可以配置信息。
```
### 局部页面

``` bash
局部页面：主题的_config.yml，打开可以配置信息。
```
### Hexo常用命令

``` bash
    hexo new "postName"       #新建文章
    hexo new page "pageName"  #新建页面
    hexo generate             #生成静态页面至public目录
    hexo server               #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
    hexo deploy               #将.deploy目录部署到GitHub
```

### Hexo命令简写

``` bash
    hexo n == hexo new
    hexo g == hexo generate
    hexo s == hexo server
    hexo d == hexo deploy
```
### 复合命令
``` bash
	hexo deploy -g
	hexo server -g
```

### 清除res
有时候生成的网页出错了，而生成的rss其实没有清除，那么用下面的命令，再重新生成:
``` bash
 hexo clean //当本地调试出现诡异现象时候，请先使用 hexo clean 清理已经生成的静态文件后重试。
 (经常更改css的时候不生效时clean然后删除.deploy_git文件夹，重新编译部署就ok)
```
## 一些报错处理

### 1.ERROR Plugin load failed: hexo-server
原因：Besides, utilities are separated into a standalone module. hexo.util is not reachable anymore.

``` bash
解决方法，执行命令：
   sudo npm install hexo-server
```
### 2.执行命令hexo server，有如下提示：
	Usage: hexo
	….
	原因：认为是没有生成本地服务
``` bash
解决方法，执行命令：
npm install hexo-server --save
提示：hexo-server@0.1.2 node_modules/hexo-server
```
### 3.白板和Cannot GET / 几个字
	原因:由于2.6以后就更新了，我们需要手动配置些东西，我们需要输入下面三行命令：

``` bash
    npm install hexo-renderer-ejs --save
    npm install hexo-renderer-stylus --save
    npm install hexo-renderer-marked --save
    这个时候再重新生成静态文件，命令：hexo generate （或hexo g）
    启动本地服务器：hexo server （或hexo s）

```
## 博客管理

``` bash
生成文章，可以直接把写好的文章插入到目录/_posts 下面，后缀为.MD就行，在文章头部固定格式：
    title: Mac提高使用效率的一些方法               #文章的标题，这个才是显示的文章标题，其实文件名不影响
    date: 2015-09-01 20:33:26     #用命令会自动生成，也可以自己写，所以文章时间可以改
    categories: technology        #文章的分类，这个可以自己定义
    tags: [Mac,效率,快捷方式]         #tag，为文章添加标签，方便搜索
```

## 插件

### 1.安装插件

``` bash
 $ npm install <plugin-name> --save
```

### 2.添加RSS

``` bash
 - npm install hexo-generator-feed
 - 到博客目录 /public 下，如果没有发现atom.xml，说明命令没有生效！！！
   解决方法：$ npm install hexo-generator-feed --save
```

### 3.在 Hexo 根目录下的 _config.yml 里配置一下，
在 hexo generate之后，会发现public文件夹下多了atom.xml！

``` bash
    feed:
        type: atom
        path: atom.xml
        limit: 20
    #type 表示类型, 是 atom 还是 rss2.
    #path 表示 Feed 路径
    #limit 最多多少篇最近文章
```

### 4.添加Sitemap
Sitemap 的提交主要的目的，是要避免搜索引擎的爬虫没有完整的收录整个网页的内容，所以提交 Sitemap 是能够补足搜索引擎的不足，
进而加速网页的收录速度，达到搜寻引擎友好的目的。


``` bash
 $ npm install hexo-generator-sitemap --save
 在 Hexo 根目录下的 _config.yml 里配置一下:
sitemap:
    path: sitemap.xml
    #path 表示 Sitemap 的路径. 默认为 sitemap.xml.
对于国内用户还需要安装插件 hexo-generator-baidu-sitemap, 顾名思义是为百度量身打造的. 安装: $ npm install hexo-generator-baidu-sitemap --save
 然后在 Hexo 根目录下的 _config.yml 里配置一下:
    baidusitemap:
        path: baidusitemap.xml
```
### 添加图片
{% asset_img xxx.png 图片介绍 %}
其中 `xxx` 是与文件同名文件夹下的图片名字
`图片介绍` 是用于展示图片干嘛用的

### 访问自己的文件
{% post_link xxx %};
`xxx` 表示访问的文件名 例如 `js-apply-call-bind`