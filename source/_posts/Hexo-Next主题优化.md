---
title: Hexo+Next主题优化
showcopyright: true
showdonate: true
date: 2017-11-07 15:00:38
type:
top:
comments:
categories: [工具,hexo]
tags: [工具,hexo]
password: qwertyuiop
---



###  置主题风格

打开 `themes/next/_config.yml` 文件，搜索  `scheme` 关键字，将你需用启用的 `scheme` 前面注释 # 去除即可。
```css
# ---------------------------------------------------------------
# Scheme Settings
# ---------------------------------------------------------------

# Schemes
#scheme: Muse # 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
#scheme: Mist # Muse 的紧凑版本，整洁有序的单栏外观
scheme: Pisces # 双栏 Scheme，小家碧玉似的清新
#scheme: Gemini # 类似 Pisces
```
<!--more-->
### 设置菜单项的显示文本和图标

NexT 使用的是 [Font Awesome](http://fontawesome.dashgame.com/)  提供的图标， [Font Awesome](http://fontawesome.dashgame.com/) 提供了 600+ 的图标，可以满足绝大的多数的场景，同时无须担心在 Retina 屏幕下图标模糊的问题。

#### 设置菜单项的显示中文文本：

打开 `themes/next/languages/zh-Hans.yml` 文件,搜索 `menu` 关键字，修改对应中文或者新增。
```css
menu:
  home: 首页
  archives: 归档
  categories: 分类
  tags: 标签
  about: 关于
  search: 搜索
  schedule: 日程表
  sitemap: 站点地图
  commonweal: 公益404
  # 新增menu
  catalogue: 目录
 ```

#### 设定菜单项的文件目录和对应图标（新版两项合并）

打开 `themes/next/_config.yml` 文件，搜索    `menu_icons` 关键字，修改对应图标名称或者新增对应 `menu` 的图标。
```css
# ---------------------------------------------------------------
# Menu Settings
# ---------------------------------------------------------------

# When running the site in a subdirectory (e.g. domain.tld/blog), remove the leading slash from link value (/archives -> archives).
# Usage: `Key: /link/ || icon`
# Key is the name of menu item. If translate for this menu will find in languages - this translate will be loaded; if not - Key name will be used. Key is case-senstive.
# Value before `||` delimeter is the target link.
# Value after `||` delimeter is the name of FontAwesome icon. If icon (with or without delimeter) is not specified, question icon will be loaded.
menu:
  home: / || home
  archives: /archives/ || history
  categories: /categories/ || list
  tags: /tags/ || tags
  tools: /categories/工具资源/ || briefcase
  about: /about/ || user
  #schedule: /schedule/ || calendar
  #sitemap: /sitemap.xml || sitemap
  #commonweal: /404/ || heartbeat

# Enable/Disable menu icons.
# Icon Mapping:
#   Map a menu item to a specific FontAwesome icon name.
#   Key is the name of menu item and value is the name of FontAwesome icon. Key is case-senstive.
#   When an question mask icon presenting up means that the item has no mapping icon.
menu_icons:
  enable: true
 ```
 除了 `home`， `archives` , `/`后面都需要手动创建这个页面

#### 创建菜单项对应文件目录,以分类为例

在终端窗口下，定位到 `Hexo` 站点目录下。使用 `hexo new page` 新建一个页面，命名为 categories ：
```sh
$ cd your-hexo-site
$ hexo new page categories
```

编辑刚新建的页面,设置分类
```css
---
title: 分类
date: 2014-12-22 12:39:04
categories: Testing #分类名
type: "categories"
---
```
### 头像设置

#### 添加头像

打开 `themes/next/_config.yml` 文件，搜索  `Sidebar Avatar` 关键字，去掉 `avatar` 前面的`#`：
```JavaScript
# Sidebar Avatar
# in theme directory(source/images): /images/avatar.jpg
# in site  directory(source/uploads): /uploads/avatar.jpg
avatar: http://example.com/avatar.png
```
或者使用本地图片,把图片放入 `themes/next/source/images` 下,修改 `avatar`：
```
avatar: /images/avatar.gif
```
#### 设置头像边框为圆形框

打开位于 `themes/next/source/css/_common/components/sidebar/sidebar-author.syl` 文件,修改如下:
```css
.site-author-image {
  display: block;
  margin: 0 auto;
  padding: $site-author-image-padding;
  max-width: $site-author-image-width;
  height: $site-author-image-height;
  border: $site-author-image-border-width solid $site-author-image-border-color;
 // 修改头像边框
  border-radius: 50%;
  -webkit-border-radius: 50%;
  -moz-border-radius: 50%;
}
```

#### 特效：鼠标放置头像上旋转

```css
.site-author-image {
  display: block;
  margin: 0 auto;
  padding: $site-author-image-padding;
  max-width: $site-author-image-width;
  height: $site-author-image-height;
  border: $site-author-image-border-width solid $site-author-image-border-color;
 // 修改头像边框
  border-radius: 50%;
  -webkit-border-radius: 50%;
  -moz-border-radius: 50%;
  // 设置旋转
  transition: 1.4s all;
}
// 可旋转的圆形头像,`hover`动作
.site-author-image:hover {
    -webkit-transform: rotate(360deg);
    -moz-transform: rotate(360deg);
    -ms-transform: rotate(360deg);
    -transform: rotate(360deg);
}
```

###  浏览页面的时候显示当前浏览进度

打开 `themes/next/_config.yml` ,搜索关键字 `scrollpercent` ,把 `false` 改为 `true`。
```css
  # Scroll percent label in b2t button
  scrollpercent: true
```
如果想把 `top`按钮放在侧边栏,打开 `themes/next/_config.yml` ,搜索关键字 `b2t` ,把 `false` 改为 `true`。
```css
 # Back to top in sidebar
  b2t: true

  # Scroll percent label in b2t button
  scrollpercent: true
```

### 侧边栏设置

#### 设置侧边栏社交链接

打开 `themes/next/_config.yml` 文件,搜索关键字 `social` ,然后添加社交站点名称与地址即可。

```css
# ---------------------------------------------------------------
# Sidebar Settings
# ---------------------------------------------------------------

# Social Links.
# Usage: `Key: permalink || icon`
# Key is the link label showing to end users.
# Value before `||` delimeter is the target permalink.
# Value after `||` delimeter is the name of FontAwesome icon. If icon (with or without delimeter) is not specified, globe icon will be loaded.
social:
  E-Mail: mailto:yourname@gmail.com || envelope
  Google: https://plus.google.com/yourname || google
  Twitter: https://twitter.com/yourname || twitter
  FB Page: https://www.facebook.com/yourname || facebook
  # 等等
```

#### 设置侧边栏社交图标

打开 `themes/next/_config.yml` 文件,搜索关键字 `social_icons` ，添加社交站点名称（注意大小写）图标，[Font Awesome](http://fontawesome.dashgame.com)图标地。

#### RSS

在你 `Hexo` 站点目录下：
```sh
$ npm install hexo-generator-feed --save
```
打开 `Hexo` 站点下的 `_config.yml` ,添加如下配置：
```JavaScript
# feed
# Dependencies: https://github.com/hexojs/hexo-generator-feed
feed:
  type: atom
  path: atom.xml
  limit: 20
  hub:
  content:
 ```

#### 友情链接

打开 `themes/next/_config.yml` 文件,搜索关键字 `Blog rolls`：
```JavaScript
# Blog rolls
links_title: 友情链接 #标题
links_layout: block #布局，一行一个连接
#links_layout: inline
links: #连接
  baidu: http://example.com/
  google: http://example.com/
```

### 主页文章添加边框阴影效果


打开 `themes/next/source/css/_custom/custom.styl` ,向里面加代码:
```JavaScript
// 主页文章添加阴影效果
.post {
   margin-top: 0px;
   margin-bottom: 60px;
   padding: 25px;
   -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
   -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
}
```

### 修改文章间分割线

打开 `themes/next/source/css/_common/components/post/post-eof.styl` ,修改：
```JavaScript
.posts-expand {
  .post-eof {
    display: block;
  //  margin: $post-eof-margin-top auto $post-eof-margin-bottom;  
    width: 0%; //分割线长度
    height: 0px; // 分割线高度
    background: $grey-light;
    text-align: center;
  }
}
```

### 代码块自定义样式

```JavaScript
// Custom styles.
code {
    color: #ff7600;
    background: #fbf7f8;
    margin: 2px;
}
// 边框的自定义样式
.highlight, pre {
    margin: 5px 0;
    padding: 5px;
    border-radius: 3px;
}
.highlight, code, pre {
    border: 1px solid #d6d6d6;
}
```

### 开启版权声明

主题配置文件下,搜索关键字 `post_copyright` , `enable` 改为 `true`：
```JavaScript
# Declare license on posts
post_copyright:
  enable: true
  license: CC BY-NC-SA 4.0
  license_url: https://creativecommons.org/licenses/by-nc-sa/4.0/
```

### 自定义文章底部版权声明

效果：
```JavaScript
作者：Dragonstyle
链接：http://www.dragonstyle.win/2017/09/06/Android-Studio个人设置/
來源：简书
版权声明： 本博客所有文章除特别声明外，均采用 CC BY-NC-SA 4.0 许可协议。转载请注明出处！
```

在目录 `themes/next/layout/_macro/` 下添加 `my-copyright.swig` ,内容如下:
```JavaScript
{% if page.copyright %}
<div class="my_post_copyright">
  <script src="//cdn.bootcss.com/clipboard.js/1.5.10/clipboard.min.js"></script>

  <!-- JS库 sweetalert 可修改路径 -->
  <script type="text/javascript" src="http://jslibs.wuxubj.cn/sweetalert_mini/jquery-1.7.1.min.js"></script>
  <script src="http://jslibs.wuxubj.cn/sweetalert_mini/sweetalert.min.js"></script>
  <link rel="stylesheet" type="text/css" href="http://jslibs.wuxubj.cn/sweetalert_mini/sweetalert.mini.css">

  <p><span>本文标题:</span>{{ page.title }}</a></p>
  <p><span>文章作者:</span>{{ theme.author }}</a></p>
  <p><span>发布时间:</span>{{ page.date.format("YYYY年MM月DD日 - HH:mm:ss") }}</p>
  <p><span>最后更新:</span>{{ page.updated.format("YYYY年MM月DD日 - HH:mm:ss") }}</p>
  <p><span>原始链接:</span><a href="{{ url_for(page.path) }}" title="{{ page.title }}">{{ page.permalink }}</a>
    <span class="copy-path"  title="点击复制文章链接"><i class="fa fa-clipboard" data-clipboard-text="{{ page.permalink }}"  aria-label="复制成功！"></i></span>
  </p>
  <p><span>许可协议:</span><i class="fa fa-creative-commons"></i> <a rel="license" href="https://creativecommons.org/licenses/by-nc-nd/4.0/" target="_blank" title="Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)">署名-非商业性使用-禁止演绎 4.0 国际</a> 转载请保留原文链接及作者。</p>  
</div>
<script>
    var clipboard = new Clipboard('.fa-clipboard');
    clipboard.on('success', $(function(){
      $(".fa-clipboard").click(function(){
        swal({   
          title: "",   
          text: '复制成功',   
          html: false,
          timer: 500,   
          showConfirmButton: false
        });
      });
    }));  
</script>
{% endif %}
```

在目录 `themes/next/source/css/_common/components/post/` 下添加 `my-post-copyright.styl`,内容如下:
```JavaScript
.my_post_copyright {
  width: 85%;
  max-width: 45em;
  margin: 2.8em auto 0;
  padding: 0.5em 1.0em;
  border: 1px solid #d3d3d3;
  font-size: 0.93rem;
  line-height: 1.6em;
  word-break: break-all;
  background: rgba(255,255,255,0.4);
}
.my_post_copyright p{margin:0;}
.my_post_copyright span {
  display: inline-block;
  width: 5.2em;
  color: #333333; // title color
  font-weight: bold;
}
.my_post_copyright .raw {
  margin-left: 1em;
  width: 5em;
}
.my_post_copyright a {
  color: #808080;
  border-bottom:0;
}
.my_post_copyright a:hover {
  color: #0593d3; // link color
  text-decoration: underline;
}
.my_post_copyright:hover .fa-clipboard {
  color: #000;
}
.my_post_copyright .post-url:hover {
  font-weight: normal;
}
.my_post_copyright .copy-path {
  margin-left: 1em;
  width: 1em;
  +mobile(){display:none;}
}
.my_post_copyright .copy-path:hover {
  color: #808080;
  cursor: pointer;
}
```
修改 `themes/next/layout/_macro/post.swig` ,在代码如下：
```JavaScript
{% if theme.wechat_subscriber.enabled and not is_index %}
      <div>
        {% include 'wechat-subscriber.swig' %}
      </div>
 {% endif %}
```
之前添加增加如下代码：
```JavaScript
<div>
      {% if not is_index %}
        {% include 'my-copyright.swig' %}
      {% endif %}
</div>
```
修改 `themes/next/source/css/_common/components/post/post.styl` 文件，在最后一行增加代码：
```JavaScript
@import "my-post-copyright"
```

设置新建文章自动开启

`copyright`,即新建文章自动显示自定义的版权声明,设置 `your site/scaffolds/post.md`文件
```JavaScript
---
title: {{ title }}
date: {{ date }}
tags:
type: "categories"
categories:
copyright: true #新增,开启
---
```

### 在右上角或者左上角实现fork me on github

选择样式[GitHub Ribbons](https://github.com/blog/273-github-ribbons),修改图片跳转链接,并复制文本框中的代码,将如下地方代码换为自己Github主页：
![](http://ov11eqxw3.bkt.clouddn.com/20171107Github.jpg/water.jpg)
打开 `themes/next/layout/_layout.swig` 文件，把代码复制到`<div class="headband"></div>`下面。

### 修改文章底部的那个带#号的标签

打开 `themes/next/layout/_macro/post.swig` 文件,搜索 `rel="tag">#` ,将 `#` 换成 `<i class="fa fa-tag"></i>`
```JavaScript
<div class="post-tags">
    {% for tag in post.tags %}
       <a href="{{ url_for(tag.path) }}" rel="tag"><i class="fa fa-tag"></i> {{ tag.name }}</a>
    {% endfor %}
</div>
```

### 添加顶部加载条

打开 `themes/next/_config.yml` ，搜索关键字 `pace` ,设置为 `true` ,可以更换加载样式：
```css
# Progress bar in the top during page loading.
pace: true
# Themes list:
#pace-theme-big-counter
#pace-theme-bounce
#pace-theme-barber-shop
#pace-theme-center-atom
#pace-theme-center-circle
#pace-theme-center-radar
#pace-theme-center-simple
#pace-theme-corner-indicator
#pace-theme-fill-left
#pace-theme-flash
#pace-theme-loading-bar
#pace-theme-mac-osx
#pace-theme-minimal
# For example
# pace_theme: pace-theme-center-simple
pace_theme: pace-theme-flash #替换更换样式
```
### 本地搜索

在你站点的根目录下
```
$ npm install hexo-generator-searchdb --save
```
打开 `Hexo` 站点的 `_config.yml`,添加配置
```JavaScript
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```
打开 `themes/next/_config.yml` ,搜索关键字 `local_search` ,设置为 `true`：
```JavaScript
# Local search
# Dependencies: https://github.com/flashlab/hexo-generator-search
local_search:
  enable: true
  # if auto, trigger search by changing input
  # if manual, trigger search by pressing enter key or search button
  trigger: auto
  # show top n results per article, show all results by setting to -1
  top_n_per_article: 1
```

### 修改网页底部

1. 在图标库中找到你自己喜欢的图标, 修改桃心,打开 `themes/next_config.yml` ,搜索关键字 `authoricon`,替换图标名
```
# icon between year and author @Footer
authoricon: id-card
```
2. 隐藏网页底部 `Hexo 强力驱动`

打开主题配置文件,搜索关键字 `copyright` ，如下:
```
# Footer `powered-by` and `theme-info` copyright
copyright: false
```
### 博文置顶
~~打开 `Hexo` 站点下 `node_modules/hexo-generator-index/lib/generator.js` 文件。代码全部替换为：~~(next 5.1以后主题已自带此功能)
```JavaScript
'use strict';
var pagination = require('hexo-pagination');
module.exports = function(locals){
  var config = this.config;
  var posts = locals.posts;
    posts.data = posts.data.sort(function(a, b) {
        if(a.top && b.top) { // 两篇文章top都有定义
            if(a.top == b.top) return b.date - a.date; // 若top值一样则按照文章日期降序排
            else return b.top - a.top; // 否则按照top值降序排
        }
        else if(a.top && !b.top) { // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
            return -1;
        }
        else if(!a.top && b.top) {
            return 1;
        }
        else return b.date - a.date; // 都没定义按照文章日期降序排
    });
  var paginationDir = config.pagination_dir || 'page';
  return pagination('', posts, {
    perPage: config.index_generator.per_page,
    layout: ['index', 'archive'],
    format: paginationDir + '/%d/',
    data: {
      __index: true
    }
  });
};
```
打开文章添加top字段,设置数值，数值越大文章越靠前：
```JavaScript
---
layout: layout
title: 标签1
date: 2017-08-18 15:41:18
tags: 标签1
top: 100
---
```

###  统计功能，统计功能,显示文章字数统计,阅读时长,总字数

在站点的根目录下：
```
$ npm i --save hexo-wordcount
```
打开 `themes/next/_config.yml` ，搜索关键字 `post_wordcount`：
```JavaScript
# Post wordcount display settings
# Dependencies: https://github.com/willin/hexo-wordcount
post_wordcount:
  item_text: true
  #字数统计
  wordcount: true
  #预览时间
  min2read: true
  #总字数,显示在页面底部
  totalcount: true
  separated_meta: true
```

### 修改文章内文本连接样式

打开 `themes/next/source/css/_custom/custom.styl`,添加代码：
```JavaScript
// 文章内链接文本样式
.post-body p a{
  color: #0593d3;
  border-bottom: none;
  border-bottom: 1px solid #0593d3;
  &:hover {
    color: #fc6423;
    border-bottom: none;
    border-bottom: 1px solid #fc6423;
  }
}
```

### 每篇文章末尾统一添加“本文结束”标记

在路径 `/themes/next/layout/_macro` 中新建  `passage-end-tag.swig` 文件,并添加以下内容：
```JavaScript
<div>
    {% if not is_index %}
        <div style="text-align:center;color: #ccc;font-size:14px;">------ 本文结束------</div>
    {% endif %}
</div>
```
打开 `themes/next/layout/_macro/post.swig` 文件,添加：
```JavaScript
<div>
    {% if not is_index %}
    {% include 'passage-end-tag.swig' %}
    {% endif %}
 </div>
```
然后打开主题配置文件 `_config.yml`,在末尾添加：
```JavaScript
# 文章末尾添加“本文结束”标记
passage_end_tag:
enabled: true
```

### 文章顶部显示更新时间

打开主题配置文件 `_config.yml` ,搜索关键字 `updated_at` 设置为 `true` ：
```JavaScript
# Post meta display settings
post_meta:
  item_text: true
  created_at: true
  updated_at: true
  categories: true
```
~~编辑文章,增加关键字`updated`~~（next可以根据文章改变时间自动更改）
```JavaScript
---
layout: layout
title: 关于
date: 2017-08-18 15:41:18
updated: 2017-09-05 20:18:54 #手动添加更新时间
```

### 修改访问URL路径

默认情况下访问URL路径为：`domain/2017/08/18/关于本站`,修改为 `domain/About/关于本站`。
编辑 `Hexo` 站点下的 `_config.yml` 文件，修改其中的 `permalink` 字段：
```JavaScript
permalink: :category/:title/
```

### 给代码块添加复制功能

- 下载插件[clipboard.js](https://github.com/zenorocha/clipboard.js) 。
- 打开 `themes/next/source/lib/` ,新建文件夹 `clipboard`。
- 把下载 `clipboard.js`下的 `src` 文件夹下的文件拖动到 `clipboard`文件夹下。
- 打开 `themes/next/source/js/src/` ,新建文件 `custom.js` ,代码如下:
```JavaScript
//此函数用于创建复制按钮
function createCopyBtns() {
    var $codeArea = $("figure table");
    //查看页面是否具有代码区域，没有代码块则不创建 复制按钮
    if ($codeArea.length > 0) {
        //复制成功后将要干的事情
        function changeToSuccess(item) {
             $imgOK = $("#copyBtn").find("#imgSuccess");
                if ($imgOK.css("display") == "none") {
                    $imgOK.css({
                        opacity: 0,
                        display: "block"
                    });
                    $imgOK.animate({
                        opacity: 1
                    }, 1000);
                    setTimeout(function() {
                        $imgOK.animate({
                            opacity: 0
                        }, 2000);
                    }, 2000);
                    setTimeout(function() {
                        $imgOK.css("display", "none");
                    }, 4000);
                };
        };
        //创建 全局复制按钮，仅有一组。包含：复制按钮，复制成功响应按钮
        //值得注意的是：1.按钮默认隐藏，2.位置使用绝对位置 position: absolute; (position: fixed 也可以，需要修改代码)
        $(".post-body").before('<div id="copyBtn" style="opacity: 0; position: absolute;top:0px;display: none;line-height: 1; font-size:1.5em"><span id="imgCopy" ><i class="fa fa-paste fa-fw"></i></span><span id="imgSuccess" style="display: none;"><i class="fa fa-check-circle fa-fw" aria-hidden="true"></i></span>');
        //创建 复制 插件，绑定单机时间到 指定元素，支持JQuery
        var clipboard = new Clipboard('#copyBtn', {
            target: function() {
                //返回需要复制的元素内容
                return document.querySelector("[copyFlag]");
            },
            isSupported: function() {
                //支持复制内容
                return document.querySelector("[copyFlag]");
            }
        });
        //复制成功事件绑定
        clipboard.on('success',
            function(e) {
                //清除内容被选择状态
                e.clearSelection();
                changeToSuccess(e);
            });
        //复制失败绑定事件
        clipboard.on('error',
            function(e) {
                console.error('Action:', e.action);
                console.error('Trigger:', e.trigger);
            });
        //鼠标 在复制按钮上滑动和离开后渐变显示/隐藏效果
        $("#copyBtn").hover(
            function() {
                $(this).stop();
                $(this).css("opacity", 1);
            },
            function() {
                $(this).animate({
                    opacity: 0
                }, 2000);
            }
        );
    }
}
//感应鼠标是否在代码区
$("figure").hover(
    function() {
        //-------鼠标活动在代码块内
        //移除之前含有复制标志代码块的 copyFlag
        $("[copyFlag]").removeAttr("copyFlag");
        //在新的（当前鼠标所在代码区）代码块插入标志：copyFlag
        $(this).find(".code").attr("copyFlag", 1);
        //获取复制按钮
        $copyBtn = $("#copyBtn");
        if ($copyBtn.lenght != 0) {
            //获取到按钮的前提下进行一下操作
            //停止按钮动画效果
            //设置为 显示状态
            //修改 复制按钮 位置到 当前代码块开始部位
            //设置代码块 左侧位置
            $copyBtn.stop();
            $copyBtn.css("opacity", 0.8);
            $copyBtn.css("display", "block");
            $copyBtn.css("top", parseInt($copyBtn.css("top")) + $(this).offset().top - $copyBtn.offset().top + 3);
            $copyBtn.css("left", -$copyBtn.width() - 3);
        }
    },
    function() {
        //-------鼠标离开代码块
        //设置复制按钮可见度 2秒内到 0
        $("#copyBtn").animate({
            opacity: 0
        }, 2000);
    }
);
//页面载入完成后，创建复制按钮
$(document).ready(function() {
  createCopyBtns();
});
```
- 打开 `themes/next/layout/_custom/` ,新建文件 `custom.swig` ，代码如下:
```JavaScript
<script type="text/javascript" src="/lib/clipboard/clipboard.js"></script>
<script type="text/javascript" src="/js/src/custom.js"></script>
```
- 修改文件 `themes/next/layout/_layout.swig` ,在标签 `</body>`上面插入代码:
```JavaScript
{% include '_custom/custom.swig' %}
```

### 新建404界面

在站点根目录下,输入 `hexo new page 404` ,默认在 `Hexo` 站点下`/source/404/index.md`
打开新建的404界面，在顶部插入一行，写上 `permalink: /404` ，这表示指定该页固定链接为 ` http://"主页"/404.html`。
```JavaScript
---
title: #404 Not Found：该页无法显示
date: 2017-09-06 15:37:18
comments: false
permalink: /404
---
```
如果你不想编辑属于自己的404界面,可以显示腾讯公益404界面,代码如下：
```html
<!DOCTYPE HTML>
<html>
<head>
  <meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="robots" content="all" />
  <meta name="robots" content="index,follow"/>
  <link rel="stylesheet" type="text/css" href="https://qzone.qq.com/gy/404/style/404style.css">
</head>
<body>
  <script type="text/plain" src="http://www.qq.com/404/search_children.js"
          charset="utf-8" homePageUrl="/"
          homePageName="回到我的主页">
  </script>
  <script src="https://qzone.qq.com/gy/404/data.js" charset="utf-8"></script>
  <script src="https://qzone.qq.com/gy/404/page.js" charset="utf-8"></script>
</body>
</html>
```
### 静态资源压缩

在站点目录下：
```
$ npm install gulp -g
```
安装gulp插件：
```
npm install gulp-minify-css --save
npm install gulp-uglify --save
npm install gulp-htmlmin --save
npm install gulp-htmlclean --save
npm install gulp-imagemin --save
```
在 `Hexo` 站点下添加 `gulpfile.js`文件，文件内容如下：
```JavaScript
var gulp = require('gulp');
var minifycss = require('gulp-minify-css');
var uglify = require('gulp-uglify');
var htmlmin = require('gulp-htmlmin');
var htmlclean = require('gulp-htmlclean');
var imagemin = require('gulp-imagemin');
// 压缩css文件
gulp.task('minify-css', function() {
  return gulp.src('./public/**/*.css')
  .pipe(minifycss())
  .pipe(gulp.dest('./public'));
});
// 压缩html文件
gulp.task('minify-html', function() {
  return gulp.src('./public/**/*.html')
  .pipe(htmlclean())
  .pipe(htmlmin({
    removeComments: true,
    minifyJS: true,
    minifyCSS: true,
    minifyURLs: true,
  }))
  .pipe(gulp.dest('./public'))
});
// 压缩js文件
gulp.task('minify-js', function() {
    return gulp.src(['./public/**/.js','!./public/js/**/*min.js'])
        .pipe(uglify())
        .pipe(gulp.dest('./public'));
});
// 压缩 public/demo 目录内图片
gulp.task('minify-images', function() {
    gulp.src('./public/demo/**/*.*')
        .pipe(imagemin({
           optimizationLevel: 5, //类型：Number  默认：3  取值范围：0-7（优化等级）
           progressive: true, //类型：Boolean 默认：false 无损压缩jpg图片
           interlaced: false, //类型：Boolean 默认：false 隔行扫描gif进行渲染
           multipass: false, //类型：Boolean 默认：false 多次优化svg直到完全优化
        }))
        .pipe(gulp.dest('./public/uploads'));
});
// 默认任务
gulp.task('default', [
  'minify-html','minify-css','minify-js','minify-images'
]);
```
只需要每次在执行 `generate` 命令后执行 `gulp ` 就可以实现对静态资源的压缩，压缩完成后执行 `deploy` 命令同步到服务器：
```
hexo g
gulp
hexo d
```

### 本地站点推送到GitHub上

在站点更目录下：
```
$ npm install hexo-deployer-git --save
```
在 `Hexo` 站点的 `_config.yml` 中配置 `deploy`：
```JavaScript
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: <repository url> #your github.io.git
  branch: master
```
```
$ hexo clean
```
```
$ hexo d --g
```
hexo g  # 生成本地 public 静态文件,
hexo d  # 部署到 Github 上,
也可以缩写成：hexo g --d 。

### 添加文章书写样式

#### 文字增加背景色块

打开 `themes/next/source/css/_custom` 下的 ` custom.styl` 文件,添加属性样式：
```css
// 颜色块-黄
span#inline-yellow {
display:inline;
padding:.2em .6em .3em;
font-size:80%;
font-weight:bold;
line-height:1;
color:#fff;
text-align:center;
white-space:nowrap;
vertical-align:baseline;
border-radius:0;
background-color: #f0ad4e;
}
// 颜色块-绿
span#inline-green {
display:inline;
padding:.2em .6em .3em;
font-size:80%;
font-weight:bold;
line-height:1;
color:#fff;
text-align:center;
white-space:nowrap;
vertical-align:baseline;
border-radius:0;
background-color: #5cb85c;
}
// 颜色块-蓝
span#inline-blue {
display:inline;
padding:.2em .6em .3em;
font-size:80%;
font-weight:bold;
line-height:1;
color:#fff;
text-align:center;
white-space:nowrap;
vertical-align:baseline;
border-radius:0;
background-color: #2780e3;
}
// 颜色块-紫
span#inline-purple {
display:inline;
padding:.2em .6em .3em;
font-size:80%;
font-weight:bold;
line-height:1;
color:#fff;
text-align:center;
white-space:nowrap;
vertical-align:baseline;
border-radius:0;
background-color: #9954bb;
}
```
在你需要编辑的文章地方。放置如下代码：
```css
<span id="inline-blue"> 站点配置文件 </span>
<span id="inline-purple"> 主题配置文件 </span>
<span id="inline-yellow"> 站点配置文件 </span>
<span id="inline-green"> 主题配置文件 </span>
```

#### 下载样式

打开 `themes/next/source/css/_custom/custom.styl` 文件,添加属性样式：
```css
a#download {
display: inline-block;
padding: 0 10px;
color: #000;
background: transparent;
border: 2px solid #000;
border-radius: 2px;
transition: all .5s ease;
font-weight: bold;
&:hover {
background: #000;
color: #fff;
}
}
```
在你需要编辑的文章地方。放置如下代码：
```css
<a id="download" href="https://git-scm.com/download/win"><i class="fa fa-download"></i><span> Download Now</span> </a>
```
#### 在文档中增加图标, [Font Awesome](http://fontawesome.dashgame.com/) 提供图标
```
<i class="fa fa-pencil"></i>支持Markdown
```

### 实现点击出现桃心效果

- 复制[网页](http://7u2ss1.com1.z0.glb.clouddn.com/love.js)代码
- 新建 `love.js` 文件并且将代码复制进去，然后保存。
- 将 `love.js`文件放到路径 `/themes/next/source/js/src` 里面
- 然后打开 `\themes\next\layout\_layout.swig` 文件,在末尾（在前面引用会出现找不到的bug）添加以下代码：
```
<!-- 页面点击小红心 -->
<script type="text/javascript" src="/js/src/love.js"></script>
```

### 添加热度


![](http://ov11eqxw3.bkt.clouddn.com/20171107redu.jpg/water.jpg)

next主题集成leanCloud,根据[next官方文档](http://theme-next.iissnan.com/third-party-services.html)设置阅读次数统计（LeanCloud) ，然后打开  `/themes/next/layout/_macro/post.swig` ,在画红线的区域添加 `℃`：
![](http://ov11eqxw3.bkt.clouddn.com/20171107redu2.jpg/water.jpg)

然后打开 `/themes/next/languages/zh-Hans.yml` ,将 `visitors` 汉化为热度就可以了：

```JavaScript
post:
  created: 创建于
  modified: 更新于
  sticky: 置顶
  posted: 发表于
  in: 分类于
  read_more: 阅读全文
  untitled: 未命名
  toc_empty: 此文章未包含目录
  visitors: 热度
  wordcount: 字数统计
  min2read: 阅读时长
  totalcount: 博客全站字数
```

### 添加 README.md 文件

每个项目下一般都有一个 `README.md` 文件，但是使用 `hexo` 部署到仓库后，项目下是没有 README.md 文件的。

在 `Hexo` 目录下的 `source` 根目录下添加一个 `README.m`d 文件，修改站点配置文件 `_config.yml` ，将 `skip_render` 参数的值设置为：
```
skip_render: README.md
```
保存退出即可。再次使用 `hexo d` 命令部署博客的时候就不会在渲染 `README.md` 这个文件了。

### 文章加密访问

打开 `themes/next/layout/_partials/head.swig`文件,在 `{% if theme.pace %}` 标签下的 `{% endif %}` 之前插入代码：

```JavaScript
  <script>
    (function(){
        if('{{ page.password }}'){
            if (prompt('请输入文章密码') !== '{{ page.password }}'){
                alert('密码错误');
                history.back();
            }
        }
    })();
</script>
```
在文章上应用：
```
---
title: 2017观看影视
date: 2017-09-25 16:10:03
type:
top:
comments:
categories: [影音, 影视]
tags: [影音, 电影, 电视剧, 动画]
password: 123456
---
```

### 添加jiathis分享

在主题配置文件中,做如下配置：
```
# Share
# This plugin is more useful in China, make sure you known how to use it.
# And you can find the use guide at official webiste: http://www.jiathis.com/.
# Warning: JiaThis does not support https.
jiathis: true
  ##uid: Get this uid from http://www.jiathis.com/
#add_this_id:
```
如果你想自定义话，打开 `themes/next/layout/_partials/share/jiathis.swig` 根据[官网](http://www.jiathis.com/)代码修改。

### 修改打赏字体不闪动

修改文件 `next/source/css/_common/components/post/post-reward.styl`，然后注释其中的函数 `wechat:hover` 和 `alipay:hover` ，如下：
```css
* 注释文字闪动函数
 #wechat:hover p{
    animation: roll 0.1s infinite linear;
    -webkit-animation: roll 0.1s infinite linear;
    -moz-animation: roll 0.1s infinite linear;
}
 #alipay:hover p{
   animation: roll 0.1s infinite linear;
    -webkit-animation: roll 0.1s infinite linear;
    -moz-animation: roll 0.1s infinite linear;
}
*/
```

### 自定义鼠标样式

打开 `themes/next/source/css/_custom/custom.styl` ,在里面写下如下代码：
```css
// 鼠标样式
  * {
      cursor: url("http://om8u46rmb.bkt.clouddn.com/sword2.ico"),auto!important
  }
  :active {
      cursor: url("http://om8u46rmb.bkt.clouddn.com/sword1.ico"),auto!important
  }
```
其中 url 里面必须是 ico 图片，ico 图片可以上传到网上（我是使用七牛云图床），然后获取外链，复制到 url 里就行了。

### 网站标题栏背景颜色

当使用Pisces主题时，网站标题栏背景颜色是黑色的，感觉不好看，可以在 `source/css/_schemes/Pisces/_brand.styl` 中修改：
```css
.site-meta {
  padding: 20px 0;
  color: white;
  background: $blue-dodger; //修改为自己喜欢的颜色

  +tablet() {
    box-shadow: 0 0 16px rgba(0,0,0,0.5);
  }
  +mobile() {
    box-shadow: 0 0 16px rgba(0,0,0,0.5);
  }
}
```
但是，我们一般不主张这样修改源码的，在 `next/source/css/_custom` 目录下面专门提供了 `custom.styl` 供我们自定义样式的，因此也可以在 `custom.styl` 里面添加：
```css
// Custom styles.
.site-meta {
  background: $blue; //修改为自己喜欢的颜色
}
```

### 修改内容区域的宽度

我们用Next主题是发现在电脑上阅读文章时内容两边留的空白较多，这样在浏览代码块时经常要滚动滚动条才能阅读完整，体验不是很好，下面提供修改内容区域的宽度的方法。
NexT 对于内容的宽度的设定如下：

- 700px，当屏幕宽度 < 1600px
- 900px，当屏幕宽度 >= 1600px
- 移动设备下，宽度自适应

如果你需要修改内容的宽度，同样需要编辑样式文件。
在Mist和Muse风格可以用下面的方法：

编辑主题的 ` source/css/_variables/custom.styl` 文件，新增变量：
```
// 修改成你期望的宽度
$content-desktop = 700px

// 当视窗超过 1600px 后的宽度
$content-desktop-large = 900px
```
当你使用Pisces风格时可以用下面的方法：
```
header{ width: 90%; }
.container .main-inner { width: 90%; }
.content-wrap { width: calc(100% - 260px); }
```

### 修改Logo字体

在 `themes/next/source/css/_custom/custom.styl`  中添加如下代码：
```css
@font-face {
    font-family: Zitiming;
    src: url('/fonts/Zitiming.ttf');
}
.site-title {
    font-size: 40px !important;
	font-family: 'Zitiming' !important;
}
```
其中字体文件在 `themes/next/source/fonts` 目录下，里面有个 `.gitkeep` 的隐藏文件，打开写入你要保留的字体文件，比如我的是就是写入 `Zitiming.ttf` ，具体字库自己从网上下载即可。

### 添加背景图

在 themes/next/source/css/_custom/custom.styl 中添加如下代码：
```css
body{
    background:url(/images/bg.jpg);
    background-size:cover;
    background-repeat:no-repeat;
    background-attachment:fixed;
    background-position:center;
}
```

### 各版块透明度修改

#### 内容板块透明

博客根目录 `themes\next\source\css\_schemes\Pisces\_layout.styl` 文件 ` .content-wrap` 标签下 `background: white`修改为：
```
background: rgba(255,255,255,0.7); //0.7是透明度
```

#### 菜单栏背景

博客根目录 `themes\next\source\css\_schemes\Pisces\_layout.styl` 文件 `.header-inner` 标签下 `background: white`修改为：
```
background: rgba(255,255,255,0.7); //0.7是透明度
```

#### 站点概况背景

博客根目录 `themes\next\source\css\_schemes\Pisces\_sidebar.styl` 文件 `.sidebar-inner` 标签下 `background: white`修改为：
```
background: rgba(255,255,255,0.7); //0.7是透明度
```
然后修改博客根目录 `themes\next\source\css\_schemes\Pisces\_layout.styl` 文件 ` .sidebar` 标签下 `background: $body-bg-color`修改为：
```
background: rgba(255,255,255,0.7); //0.7是透明度
```

#### 按钮背景

博客根目录 `themes\next\source\css\_common\components\post\post-button.styl` 同上修改对应位置为 `background: transparent;`

### 添加网易云音乐

在网易云音乐（网页版）中搜索我们想要插入的音乐，然后点击生成外链播放器
![](http://ov11eqxw3.bkt.clouddn.com/20171109wangyiyun.jpg/water.jpg)
然后根据你得设置生成相应的html代码，将获得的html代码插入到你想要插入的位置。

我放在了侧边栏，在 `themes/next/layout/_custom/sidebar.swig` 文件中增加生成的HTML代码：
```html
<div id="music163player">
    <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=280 height=86 src="//music.163.com/outchain/player?type=2&id=38358214&auto=0&height=66">
    </iframe>
</div>
```
可以根据自己实际情况修改宽高等样式。

参考自：

http://www.jianshu.com/p/3ff20be857
http://blog.csdn.net/qq_33699981/article/details/72716951
http://blog.csdn.net/heqiangflytosky/article/details/54863185
