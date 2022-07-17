title: yilia主题优化
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

### 页面压缩
js／css位置
css引用建议放在head标签里面；js脚本建议放到body内容的最后，原因：等待js加载或者脚本有错误的时候不会影响html页面的展示


``` sh
cnpm install --save-dev gulp
cnpm install gulp-htmlclean gulp-htmlmin gulp-minify-css gulp-uglify gulp-imagemin --save

# 在博客根目录下创建一个名为 gulpfile.js 的文件
// 引入需要的模块
var gulp = require('gulp');
var minifycss = require('gulp-minify-css');
var uglify = require('gulp-uglify');
var htmlmin = require('gulp-htmlmin');
var htmlclean = require('gulp-htmlclean');
var imagemin = require('gulp-imagemin');

// 压缩public目录下所有html文件, minify-html是任务名, 设置为default，启动gulp压缩的时候可以省去任务名
gulp.task('minify-html', function() {
    return gulp.src('./public/**/*.html') // 压缩文件所在的目录
        .pipe(htmlclean())
        .pipe(htmlmin({
            removeComments: true,
            minifyJS: true,
            minifyCSS: true,
            minifyURLs: true,
        }))
        .pipe(gulp.dest('./public')) // 输出的目录
});

// 压缩css
gulp.task('minify-css', function() {
    return gulp.src('./public/**/*.css')
        .pipe(minifycss({
            compatibility: 'ie8'
        }))
        .pipe(gulp.dest('./public'));
});
// 压缩js
gulp.task('minify-js', function() {
    return gulp.src(['./public/**/.js','!./public/js/**/*min.js'])
        .pipe(uglify())
        .pipe(gulp.dest('./public'));
});
// 压缩图片
gulp.task('minify-images', function() {
    return gulp.src(['./public/**/*.png','./public/**/*.jpg','./public/**/*.gif'])
        .pipe(imagemin(
        [imagemin.gifsicle({'optimizationLevel': 3}), 
        imagemin.jpegtran({'progressive': true}), 
        imagemin.optipng({'optimizationLevel': 7}), 
        imagemin.svgo()],
        {'verbose': true}))
        .pipe(gulp.dest('./public'))
});
// 默认任务
/*
gulp.task('default', [
    'minify-html','minify-html1','minify-css','minify-js','minify-images'
]);
*/
// gulp 4.0 适用的方式
gulp.task('default', gulp.parallel('minify-html','minify-css','minify-js','minify-images'
 //build the website
));

# 以min.js这样后缀名的文件再次压缩, 可能会出现错误, 所以要将它排除掉
hexo clean && generate
gulp default   
#压缩js、css、img等文件, default是上面gulpfile.js合并任务的任务名
hexo deploy
```


### 添加隐藏左边栏目按钮
``` sh
.mymenucontainer {
	display:block;
	cursor:pointer;
	left:0;
	top:0;
	width:35px;
	height:35px;
	z-index:9999;
	position:fixed;
}
.bar1 {
	width:35px;
	height:3px;
	background-color:#333;
	margin:6px 0;
	transition:0.4s;
	-webkit-transform:rotate(-45deg) translate(-8px,8px);
	transform:rotate(-45deg) translate(-8px,8px);
}
.bar2 {
	width:35px;
	height:3px;
	background-color:#333;
	margin:6px 0;
	transition:0.4s;
	opacity:0;
}
.bar3 {
	width:35px;
	height:3px;
	background-color:#333;
	margin:6px 0;
	transition:0.4s;
	-webkit-transform:rotate(45deg) translate(-4px,-6px);
	transform:rotate(45deg) translate(-4px,-6px);
}
.change .bar1 {
	-webkit-transform:rotate(0deg) translate(0px,0px);
	transform:rotate(0deg) translate(0px,0px);
}
.change .bar2 {
	opacity:1;
}
.change .bar3 {
	-webkit-transform:rotate(0deg) translate(0px,0px);
	transform:rotate(0deg) translate(0px,0px);
}
# 样式制作完成后,压缩,然后添加进\themes\yilia\source\main.0cf68a.css文件中,添加在最前面即可

# 添加按钮到相应的位置
# 打开\themes\yilia\layout\layout.ejs文件, 找到<div class="left-col",在其上面添加如下代码:
<div class="mymenucontainer" onclick="myFunction(this)">
  <div class="bar1"></div>
  <div class="bar2"></div>
  <div class="bar3"></div>
</div>
# 在</body>之后, </html>前添加如下Js代码:
<script>
    var hide = false;
    function myFunction(x) {
        x.classList.toggle("change");
        if(hide == false){
            $(".left-col").css('display', 'none');
            $(".mid-col").css("left", 6);
            $(".tools-col").css('display', 'none');
            $(".tools-col.hide").css('display', 'none');
            hide = true;
        }else{
            $(".left-col").css('display', '');
            $(".mid-col").css("left", 300);
            $(".tools-col").css('display', '');
            $(".tools-col.hide").css('display', '');
            hide = false;
        }
    }
</script>
```

### 字体优化
字体更换、压缩

``` shell
cnpm install font-spider -g

# 要确保ttf文件一定要有，其他的不管
# 在html中也引用了相应的css文件


font-spider C:\Users\wangchao\Desktop\index*.html
#html完整路径 【*】 是通配符，表示会扫描所有的html文件
```

### 短链接
`npm install hexo-abbrlink --save`
``` yml
permalink: archives/:year:month:day:abbrlink/
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
  # 下面几项可以省略
  drafts: false #(true)Process draft,(false)Do not process draft
  # Generate categories from directory-tree
  # depth: the max_depth of directory-tree you want to generate, should > 0
  auto_category:
     enable: false
     depth:
```