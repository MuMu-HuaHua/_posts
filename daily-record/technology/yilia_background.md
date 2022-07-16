title: 背景优化
date: 2022-07-16 18:30:09
categories:
- [Daily-Record,Technology]
tags:
- Hexo
---
yilia主题背景调优 && 底部标签转码
<!--more-->
### 侧栏背景
``` raw
# themes\yilia\layout\layout.ejs
# 在最上面添加 
<% var left_default = '#fff'; %>
# 作为默认值，如果你不加图片，就会默认为白色
# 在其中为它添加内联样式
<div class="left-col" q-class="show:isShow"></div>

<div class="left-col" q-class="show:isShow" style="background: <%= theme.style && theme.style.left_ground ? theme.style.left_ground : left_default %>">

# theme.style.left_ground中的left_ground就是主题配置文件_config.yml中style下所要添加的名称。


# themes\yilia\_config.yml中添加
# 头像上面的背景颜色
# 设置背景透明，不然头像上方是默认色
header: 'rgba(0,0,0,0)' 
#左侧头像板块的背景颜色
left_ground: 'url(/assets/blogImg/timg2.png)no-repeat 100%;background-size:cover;'
```

### 页面背景
``` raw
# \themes\yilia\layout下的layout.ejs文件。

# body标签内添加行内样式：
<body  style="background: url(https://w.wallhaven.cc/full/gj/wallhaven-gjlxke.png); background-size: cover;background-repeat: no-repeat;opacity: 0.85">
```
参数|说明
--|--
background: url();|路径
background-size: cover;|表示图片根据屏幕大小自适应占满整个屏
background-repeat: no-repeat;|如果图片小的话会增加很多个相同的来铺满页面，no-repeat表示不平铺。
opacity: 0.85;|透明度


### 底部标签
出现转码问题
``` raw
    <nav id="page-nav">
      <%- paginator({
        prev_text: '&laquo; Prev',
        next_text: 'Next &raquo;',
	escape: false
      }) %>
    </nav>
```