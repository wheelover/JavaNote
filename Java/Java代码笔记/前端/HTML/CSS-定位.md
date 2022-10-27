#CSS-定位
css的非常关键属性-position，浏览器会自动给所有的DOM元素，添加position：static

##static：固定


##relative：相对定位
margin会引起文档流的变化，
这里我们可以left，top，right，bottom属性来实现偏移，但是在static的情况下不可以用
left：50px表示距离自己原来位置左侧50px
这些调整都是相对于当前元素static布局位置进行的调整

##absolute：绝对定位
absolute和relation的区别，relation是相对自己进行top，left，right，bottom进行偏移，而absolute是寻找最近的非static的祖先节点进行偏移
脱离文档流

##fixed：固定
固定定位和绝对定位类似，但元素的包含为屏幕视口
被图片挡住时用z-index（图层优先级）
fixed会使其脱离正常的文档流

##sticky：粘性
当他滚动到顶部时，黏在了顶部。而当页面往下面滚动是，又会让其恢复在文档中的位置

##float：浮动
是css中最常用的布局属性，使用它可以让元素靠左或靠右排版
float：left/right（靠向左或向右）


#实战
##模态框
###半透明背景添加:
通过fixed，设置mask是撑满整个屏幕的
```
.mask {
  position: fixed; /*1*/
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.7); /*2*/
}
```
###元素水平居中：
1. 如果是行内元素，我们可以在父容器上使用text-align：center
2. 如果内部是块状元素，我们可以在子容器上使用margin：0 auto

###元素垂直居中
flex


##搜索框
```html
html:
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="stylesheet" type="text/css" href="./index.css" />
    <title>知乎</title>
  </head>
  <body>
    <nav class="nav">
      <div class="content clearFixed">
        <div class="left">
          <img
            class="logo"
            src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/zhihu/zhihu.png"
          />
          <ul class="menu">
            <li class="active">
              首页
              <div class="bottom"></div>
            </li>
            <li>发现</li>
            <li>等你来答</li>
          </ul>
          <div class="search">
            <input type="text" placeholder="美妆博主" />
            <img
              class="search-icon"
              src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/zhihu/search.png"
            />
          </div>
          <button class="ask-btn">提问</button>
        </div>
        <ul class="right">
          <li class="avatar">
            <img
              src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/zhihu/%E4%BD%8D%E5%9B%BE.png"
            />
          </li>
          <li class="message">
            <img
              src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/zhihu/%E6%B6%88%E6%81%AF.png"
            />
          </li>
          <li class="notice">
            <img
              src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/zhihu/%E9%80%9A%E7%9F%A5.png"
            />
          </li>
        </ul>
        <div class="setting">
          <ul>
            <li>
              <img
                src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/zhihu/%E4%B8%AA%E4%BA%BA.png"
              />
              <div>我的主页</div>
            </li>
            <li>
              <img
                src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/zhihu/%E8%AE%BE%E7%BD%AE.png"
              />
              <div>设置</div>
            </li>
            <li class="last">
              <img
                src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/zhihu/%E9%80%80%E5%87%BA.png"
              />
              <div>退出</div>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </body>
</html>

css:
html,
body {
  height: 100%;
  margin: 0;
}

ul,
li {
  margin: 0;
  padding: 0;
}
li {
  list-style: none;
}

h1,
h2 {
  margin: 0;
}

p {
  margin: 0;
  padding: 0;
}

.clearFixed::after {
  content: '';
  display: block;
  clear: both;
}

/* 浏览器都有自己的默认样式，以上部分为清除浏览器默认样式 */

.nav {
  height: 54px;
  border-bottom: 1px solid #eeeeee;
}

/* 中间区域 */
.content {
  position: relative;
  /* 下面两行代码可以保证中间区域永远左右居中 */
  width: 1000px;
  margin: 0 auto;
}

.left {
  float: left;
}

.right {
  float: right;
}

.logo {
  width: 64px;
  height: 30px;
  margin-top: 12px;
  margin-right: 40px;
  float: left;
}

ul.menu {
  float: left;
  height: 100%;
}

ul.menu li {
  height: 100%;
  float: left;
  font-weight: 500;
  font-size: 16px;
  line-height: 54px;
  color: #8892a6;
  margin-right: 30px;
}

ul.menu li.active {
  position: relative;
  color: #1c1c1c;
}

ul.menu li.active .bottom {
  position: absolute;
  bottom: 0;
  /* 下面两行代码可以使蓝色条和文字中线对齐 */
  left: 50%;
  margin-left: -15px;
  width: 30px;
  height: 3px;
  background-color: #1987fb;
}

.search {
  position: relative;
  float: left;
  width: 326px;
  height: 34px;
  margin-top: 10px;
  margin-right: 16px;
  margin-left: 10px;
}

::-webkit-input-placeholder {
  /* WebKit browsers */
  color: #8892a6;
  font-weight: 500;
  font-size: 14px;
}

.search input {
  height: 100%;
  width: 100%;
  /* 由于我们还给input设置了padding，所以需要设置box-sizing: border-box防止 input 超出到父元素外 */
  box-sizing: border-box;
  background: #f6f6f6;
  border: 1px solid #ebebeb;
  border-radius: 3px;
  padding: 7px 15px;
  font-size: 14px;
}

.search-icon {
  position: absolute;
  right: 15px;
  /* 9 = (34 - 16) / 2 */
  /* 34 是.search的高度，16 是.search-icon的高度，除以 2 是因为上下距离是一样的 */
  top: 9px;
  width: 16px;
  height: 16px;
}

.ask-btn {
  width: 58px;
  height: 34px;
  margin-top: 10px;
  text-align: center;
  font-size: 14px;
  line-height: 34px;
  color: #ffff;
  background: #0084ff;
  border: 1px solid #0084ff;
  border-radius: 3px;
}

.avatar {
  float: right;
}

/* 因为第一个设置向右浮动的元素会在最右边 */
/* 所以 html 中 li.avatar、li.message、li.notice 的顺序和页面上的顺序是正好相反的 */
ul.right li {
  float: right;
  margin-left: 36px;
  width: 26px;
  height: 26px;
  margin-top: 14px;
}

ul.right li > img {
  width: 100%;
  height: 100%;
}

ul.right li.avatar {
  width: 30px;
  height: 30px;
  margin-top: 12px;
}

.setting {
  position: absolute;
  right: -52px;
  top: 48px;
  box-sizing: border-box;
  width: 134px;
  height: 122px;
  background: #ffffff;
  border: 1px solid #ebebeb;
  border-radius: 4px;
  padding: 16px 18px 0 18px;
}

.setting ul li {
  margin-bottom: 15px;
  height: 20px;
}

.setting ul li.last {
  margin-bottom: 0;
}

.setting ul li img {
  margin-top: 2px;
  width: 16px;
  height: 16px;
  float: left;
}

.setting ul li div {
  float: left;
  height: 20px;
  font-weight: 500;
  font-size: 14px;
  line-height: 20px;
  color: #8892a6;
  margin-left: 5px;
}




```







