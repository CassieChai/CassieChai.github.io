---
title: hexo博客-优化篇
date: 2018-03-15 20:26:44
tags: [Hexo]
categories: [Hexo]
---
## 在文章中插入图片 

在source目录下新建一个img文件夹，将图片放入该文件夹下

插入图片时将链接写为`/img/图片名称` 

## 设置鼠标经过头像时头像旋转

打开`\themes\next\source\css\_common\components\sidebar\sidebar-author.styl`，在里面添加如下代码即可：

```
.site-author-image {
  display: block;
  margin: 0 auto;
  padding: $site-author-image-padding;
  max-width: $site-author-image-width;
  height: $site-author-image-height;
  border: $site-author-image-border-width solid $site-author-image-border-color;

  /* 头像圆形 */
  border-radius: 80px;
  -webkit-border-radius: 80px;
  -moz-border-radius: 80px;
  box-shadow: inset 0 -1px 0 #333sf;

  /* 设置循环动画 [animation: (play)动画名称 (2s)动画播放时长单位秒或微秒 (ase-out)动画播放的速度曲线为以低速结束 
    (1s)等待1秒然后开始动画 (1)动画播放次数(infinite为循环播放) ]*/


  /* 鼠标经过头像旋转360度 */
  -webkit-transition: -webkit-transform 1.0s ease-out;
  -moz-transition: -moz-transform 1.0s ease-out;
  transition: transform 1.0s ease-out;
}

img:hover {
  /* 鼠标经过停止头像旋转 
  -webkit-animation-play-state:paused;
  animation-play-state:paused;*/

  /* 鼠标经过头像旋转360度 */
  -webkit-transform: rotateZ(360deg);
  -moz-transform: rotateZ(360deg);
  transform: rotateZ(360deg);
}

/* Z 轴旋转动画 */
@-webkit-keyframes play {
  0% {
    -webkit-transform: rotateZ(0deg);
  }
  100% {
    -webkit-transform: rotateZ(-360deg);
  }
}
@-moz-keyframes play {
  0% {
    -moz-transform: rotateZ(0deg);
  }
  100% {
    -moz-transform: rotateZ(-360deg);
  }
}
@keyframes play {
  0% {
    transform: rotateZ(0deg);
  }
  100% {
    transform: rotateZ(-360deg);
  }
}
```

## 设置侧边栏的社交图标
打开主题配置文件`themes\next\_config.yml`，搜索`social_icons`,在图标库找自己喜欢的小图标，并将名字复制在如下位置，保存即可

![](/img/设置侧边栏图标.png)
**注意：**第一次操作时记得将`social`和**你希望添加的社交地址**前面的#去掉

## 为文章添加阴影边框
打开`\themes\next\source\css\_custom\custom.styl`,向里面加入：
```
// 主页文章添加阴影效果
 .post {
   margin-top: 60px;
   margin-bottom: 60px;
   padding: 25px;
   -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
   -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
  }
```
## 在网站底部添加访问量

打开`\themes\next\layout\_partials\footer.swig`文件,在copyright前添加```<script async src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>```和合适的位置添加显示统计的代码： 
```
<div class="powered-by">
<i class="fa fa-user-md"></i><span id="busuanzi_container_site_uv">
  本站访客数:<span id="busuanzi_value_site_uv"></span>
</span>
</div>
```
如下图所示：
![](/img/添加访问量.png)

## 为文章实现统计功能

在根目录下安装 `hexo-wordcount`,运行：

```
$ npm install hexo-wordcount --save
```

打开主题配置文件`themes\next\_config.yml`，配置如下：

```
# Post wordcount display settings
# Dependencies: https://github.com/willin/hexo-wordcount
post_wordcount:
  item_text: true
  wordcount: true
  min2read: true
```
这时我们发现文章的【字数统计】和【阅读时长】后面没有对应的xxx字，xx分钟等单位，所以接下来打开`themes\next\layout\_macro\post.swig`文件
在对应为位置添加如下字样：
![](/img/阅读时长添加单位.png)

## 刷新之后出现乱码

将文件保存为UTF-8即可解决

## 首页文章设置为只显示预览

打开主题配置文件`themes\next\_config.yml`
搜索"auto_excerpt",找到如下部分：
```
# Automatically Excerpt. Not recommand.
# Please use <!-- more --> in the post to control excerpt accurately.
auto_excerpt:
  enable: false
  length: 150
```
把enable改为对应的false改为true即可

## 在文章底部添加jiathis分享

打开主题配置文件`themes\next\_config.yml`中，设置`jiathis: true`，即打开了jiathis分享，如下图所示：

![](/img/jiathis分享.png)

想自定义话，打开`themes\next\layout\_partials\share\jiathis.swig`修改每一个部分就可以了 

```<div class="jiathis_style">
<span class="jiathis_txt">分享到：</span>
	<a class="jiathis_button_fav">收藏夹</a>
	<a class="jiathis_button_copy">复制网址</a>
	<a class="jiathis_button_email">邮件</a>
	<a class="jiathis_button_weixin">微信</a>
	<a class="jiathis_button_cqq">QQ</a>
	<a class="jiathis_button_tsina">新浪微博</a>
	<a class="jiathis_button_douban">豆瓣</a>
	<a class="jiathis_button_share">一键分享</a>
```

## 修改文章底部标签的“#”

打开`\themes\next\layout\_macro\post.swig`

搜索 `rel="tag">#`

将 # 换成`<i class="fa fa-tag"></i>`即可

