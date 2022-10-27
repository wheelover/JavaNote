#CSS伪类
```
/* before */
选择器::before{
  /* 使用空白符号占位 */
  content: '';
}

/* after */
选择器::after{
  /* 使用空白符号占位 */
  content: '';
}

```

```
<!-- 头部 -->
<header>header</header>
<!-- 主体 -->
<main>
    <!-- 导航 -->
    <nav>nav</nav>
    <!-- 区块 -->
    <section>section</section>
    <section>section</section>
</main>
<!-- 侧边栏 -->
<aside></aside>
<!-- 底部 -->
<footer></footer>
```

清除浮动
```
.clearfix::after{
  content: '';
  display: block;
  clear: both;
}
```
商品列表
```
html:
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>练习</title>
    <link rel="stylesheet" href="index.css">
</head>

<body>
    <section class="section-goods">
        <main class="main-good-list">
            <!--商品头部-->
            <header class="clearFix">
                <div class="title-left">
                    智能
                </div>
                <ul class="title-right">
                    <li class="selected">智能</li>
                    <li>安防</li>
                    <li>出行</li>
                </ul>
            </header>

            <!--商品列表-->
            <ul class="good-list clearFix">
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li class="margin-off"></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li class="margin-off"></li>
            </ul>
            <a class="book" href="#"></a>
        </main>
    </section>
</body>

</html>

css:
* {
    margin: 0px;
    padding: 0px;
    box-sizing: border-box;
}

ul,
li {
    list-style: none;
}

a {
    text-decoration: none;
}

.clearFix::after {
    content: '';
    display: block;
    clear: both;
}

.section-goods {
    width: 1439px;
    background: #F5F5F5;
}

.main-good-list {
    width: 1048px;
    margin: 0 auto;
}

/*商品头部*/
.title-left {
    float: left;
    color: #000000;
    font-size: 20px;
    line-height: 54px;
}

.title-right {
    float: right;
}

.title-right>li {
    position: relative;
    box-sizing: border-box;
    float: left;
    margin-left: 20px;
    line-height: 54px;
    color: #999999;
    font-size: 16px;
}

.title-right>li.selected {
    margin-left: 0px;
    color: #FD6821;
}

.title-right>li.selected::after {
    content: '';
    position: absolute;
    bottom: 11px;
    left: 1px;
    width: 30px;
    height: 2px;
    background: #FD6821;
}

/*商品列表*/
.good-list>li {
    float: left;
    width: 200px;
    height: 256px;
    margin-right: 12px;
    margin-bottom: 12px;
    background-color: #FFFFFF;
}

.good-list>li.margin-off {
    margin-right: 0px;
}

/*电纸书*/
.book {
    display: block;
    width: 1048px;
    height: 103px;
    margin: 0 auto;
    margin-top: 20px;
    background: url(./images/book.jpg) no-repeat center;
    background-size: contain;
}
```
事件伪类
1.hover： 鼠标移动上去
```
li:hover{
    background-color: #47A0FC;
    color: white;
}
```
2.active：鼠标点击时
```
ul>li:active{
    /* 要改变的效果 */
    color: black;
}
```
注意hover一定要在active之前，否则不会生效
3.focus获取焦点后，一般用于具有焦点的元素，比如input，可以让input获取到焦点以后，改变input的边框的颜色
```
input:focus{
	border: 1px solid salmon;
}
```
一个标签上可以添加多个：hover效果
：hover父元素，改变直接子元素的属性
如何通过父元素的hover改变直接子元素的样式
```
<div>
    <span></span>
</div>

div:hover>span{
    background:blue;
}
```
如何显示/隐藏元素
	如何通过兄弟元素的伪类改变另一个元素的属性
```
<div>
    <input type="text">
    <div></div>
</div>

/* 选中获得焦点的 input 元素后面一个 div 元素（input 和 div 是兄弟元素） */
input:focus+div{
    border:1px solid blue;
}
```
记住，被改变的元素要紧邻有伪类效果的元素
结合hover和display控制元素的显示和隐藏
```
<div class="box"></div>
<ul>
    <li>item</li>
    <li>item</li>
    <li>item</li>
</ul>

ul{
    display:none;
}
.box{
    width: 100px;
    height: 30px;
    background-color: #FF0000;
}
.box:hover+ul{
    display: block;
}
```
事件伪类实例
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>练习</title>
    <link rel="stylesheet" href="index.css" type="text/css" media="all" />
</head>

<body>
    <div class="search-box">
        <input type="text" value="红米k20">
        <div class="select-list">
            <ul>
                <li>小米9</li>
                <li>Redmi K20 pro</li>
                <li>Redmi K20 </li>
                <li>Redmi note 7 Pro </li>
                <li>Redmi note 7</li>
                <li>小米电视4c</li>
                <li>电视32英寸</li>
                <li>笔记本Pro</li>
                <li>小爱音箱</li>
                <li>净水器</li>
            </ul>
        </div>
        <div class="search-btn">
            <span></span>
        </div>

    </div>
</body>

</html>

body {
    margin: 0;
    background-color: #fff;
}

ul,
li {
    margin: 0px;
    padding: 0px;
    list-style: none;
}

.search-box {
    position: relative;
    box-sizing: border-box;
    width: 254px;
    height: 42px;
    background: #FFFFFF;
    border: 1px solid #E0E0E0;
}

.search-box>input {
    box-sizing: border-box;
    float: left;
    border: none;
    height: 40px;
    width: 209px;
    padding-left: 20px;
    outline: none;
    font-style: normal;
    font-weight: normal;
    font-size: 12px;
    line-height: 17px;
    color: #AEAEAE;
}

.search-box .search-btn {
    float: right;
    height: 42px;
    width: 42px;
    border: 1px solid #E0E0E0;
    box-sizing: border-box;
    margin-top: -1px;
    margin-right: -1px;
    padding: 12px;
}

.search-box>div>span {
    display: block;
    width: 16px;
    height: 16px;
    background: url(./images/search-normal.png) no-repeat center;
    background-size: contain;
}

.search-box .select-list {
    position: absolute;
    top: 40px;
    left: -1px;
    width: 213px;
    height: 282px;
    background: #FFFFFF;
    border: 1px solid #FD6821;
    box-sizing: border-box;
    display: none;
}

.search-box .select-list li {
    height: 28px;
    font-weight: normal;
    font-size: 12px;
    color: #000000;
    padding-left: 15px;
    line-height: 28px;
}

3
.search-box:hover {
    border: 1px solid #FD6821;
}

.search-box:hover .search-btn {
    border: 1px solid #FD6821;
}

.search-box input:focus+.select-list {
    display: block;
}
```

列表伪类
:first-child 配其父元素中的第一个子元素
```
ul>li:first-child{
    background-color: #3687FC;
    color: #FFFFFF;
}
```
:last-child 配其父元素中的最后子元素

:nth-child() 匹配其父元素的第n个子元素

列表伪类的使用条件是，同一级别，相同元素

注：要用first-child的话，这种情况父元素下的第一个子元素一定要是你要设置伪类的子元素标签，否则用“p:first-of-type”