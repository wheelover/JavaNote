#移动端web开发


##meta元素：
meta是一个自闭和元素，只有开始标签，没有结束标签
```
<!-- 声明文档使用的字符编码 -->
<meta charset="utf-8">
<!-- http-equiv相当于http的文件头作用，它可以向浏览器传回一些有用的信息，以帮助浏览器正确地显示网页内容 -->
<!-- 与http-equiv对应的属性值为content，这里IE=edge告诉IE使用最新的引擎渲染网页 -->
<meta http-equiv="X-UA-Compatible" content="IE=Edge">
<!-- name属性主要用于描述网页，与之对应的属性值为content，content中的内容主要是便于搜索引擎机器人查找信息和分类信息用的 -->
<meta name="title" content="优酷-中国领先视频网站,提供视频播放,视频发布,视频搜索 - 优酷视频" />
<meta name="keywords" content="视频,视频分享,视频搜索,视频播放,优酷视频" />
<meta name="description" content="视频服务平台,提供视频播放,视频发布,视频搜索,视频分享" />
```
charset的属性：utf-8：涵盖了世界上几乎所有的字符和符号，为HTML5的默认字符集
###移动web前端meta的基础设置
在开发移动端的页面时，我们需要设置meta标签如下
```
<!-- 设置移动端视图 -->
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />
```
这里的设置表示：H5页面窗口自动调整到设备宽度，并禁止用户缩放页面
viewport的属性：
* width：viewpoint的宽度
* height：高度（很少用） 
* intital-scale：初始的缩放比例
* minimun-scale：允许用户缩放到的最小比例
* maximun-scale：允许用户缩放到的最大比例
* user-scalable：用户是否可以手动缩放

##新单位rem/em
###单位rem
在开发响应式页面时，同一个元素在不同的浏览器宽度下常常会设置不同的尺寸
rem是一个相对长度单位，在不同环境中，1rem可以转化为不同的px值
rem和px之间的转换比取决于页根元素（<html><html/>）的字体的大小。1rem = 根元素的大小
一般浏览器中根元素得我字体默认大小为16px，有一些情况会有例外
最小显示字体一般为10px，用6px也会按照10px的大小显示，除非用到了css的transform
```
// .title选中大标题“11 powerful examples of responsive web design”
// .logo选中右上角logo
@media screen and (max-width: 768px) {
  html {
    font-size: 16px;
  }
}
@media screen and (min-width: 768px) {
  html {
    font-size: 20px;
  }
}

.title {
  font-size: 2.5rem;
}
.logo {
  width: 10rem;
  height: 10rem;
}
```
###单位em
em是根据父元素字体大小转换的
```
<div class="parent">
  父元素中的文字
  <div class="child">
    子元素中的文字
  </div>
</div>
.parent {
  // 父元素的字体大小为 20px
  font-size: 20px;

  > .child {
    // 子元素的字体大小为 0.7 * 20px = 14px
    font-size: 0.7em; 
    // 特别注意：子元素的宽度为 10 * 14px = 140px
    width: 10em;
  }
}
```
子元素的em的转换分两种：
1. 子元素字体大小：0.7em，这里的1em和父元素的字体大小相同
2. 子元素的宽度：10em，这里的1em和子元素的字体大小相同
注意：多层嵌套中使用em，很容易出现计算错误


###单位vw和vh
* 1vw = 1/100视口宽度
* 1vh= 1/100视口高度
视口就是指文档的可见部分
<img src="https://document.youkeda.com/P3-3-HTML-CSS/1/3.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image text"/>
在模态框中常用到这两个单位

























