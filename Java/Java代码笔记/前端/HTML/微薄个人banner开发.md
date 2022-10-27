#微薄个人banner开发
```
html:
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>实战微博--banner</title>
    <link rel="stylesheet" href="index.css">
</head>

<body>
    <section class="banner">
        <header>
            <!-- 头像 -->
            <div class="head"></div>
            <!-- 姓名 性别 等级 -->
            <div class="title">
                <span>我是娜扎</span>
                <i class="sex"></i>
                <i class="level"></i>
            </div>
            <div class="introduce">
                演员，代表作《择天记》
            </div>
            <div class="actions clearfix">
                <div class="focus"><span>关注</span></div>
                <div class="focus-more"></div>
                <div class="p-message">私信</div>
                <div class="m-more"></div>
            </div>
        </header>
        <nav>
            <div class="container clearfix">
                <span class="left">她的主页</span>
                <span class="right">她的相册</span>
            </div>
        </nav>
    </section>
</body>

</html>

css:
* {
  margin: 0;
  padding: 0;
}

.clearfix::after {
  content: '';
  display: block;
  clear: both;
}

.banner {
  box-sizing: border-box;
  width: 920px;
}

.banner header {
  box-sizing: border-box;
  height: 300px;
  padding-top: 30px;
  background: url(./images/banner-bg.png) no-repeat center;
  background-size: contain;
}

.banner header .head {
  width: 110px;
  height: 110px;
  margin: 0 auto;
  border-radius: 50%;
  background: url(./images/banner-head-icon.png) no-repeat center;
  background-size: contain;
}

.banner header .title {
  margin-top: 10px;
  margin-bottom: 5px;
  text-align: center;
}

.banner header .title span {
  font-size: 22px;
  line-height: 29px;
  font-family: PingFang SC;
  font-style: normal;
  font-weight: 500;
  color: #ffffff;
  text-shadow: 0px 0px 4px rgba(0, 0, 0, 0.31);
  vertical-align: text-bottom;
}

.banner header .title i {
  display: inline-block;
  width: 16px;
  height: 16px;
}

.banner header .title i.sex {
  background: url(./images/sex-icon.png) no-repeat center;
  background-size: contain;
}

.banner header .title i.level {
  background: url(./images/leval-icon.png) no-repeat center;
  background-size: contain;
}

.banner header .introduce {
  margin-bottom: 19px;
  text-align: center;
  font-family: PingFang SC;
  font-style: normal;
  font-weight: 500;
  font-size: 12px;
  line-height: 20px;
  color: #ffffff;
  text-shadow: 0px 0px 6px rgba(0, 0, 0, 0.42);
}

.banner header .actions {
  width: 275px;
  margin: 0 auto;
}

.banner header .actions .focus {
  position: relative;
  float: left;
  width: 100px;
  background: linear-gradient(180deg, #fa823c 0%, #f55f10 100%);
  border-radius: 2px;
  font-family: PingFang SC;
  font-style: normal;
  font-weight: 500;
  font-size: 14px;
  line-height: 32px;
  color: #ffffff;
}

.banner header .actions .focus::before {
  position: absolute;
  top: 11px;
  left: 27px;
  content: '';
  width: 10px;
  height: 10px;
  background: url(./images/add-icon.png) no-repeat center;
  background-size: contain;
}

.banner header .actions .focus span {
  margin-left: 42px;
}

.banner header .actions .focus-more {
  position: relative;
  float: left;
  margin-left: 5px;
  margin-right: 12px;
  width: 20px;
  height: 32px;
  background: linear-gradient(180deg, #fa823c 0%, #f55f10 100%);
  border-radius: 2px;
}

.banner header .actions .focus-more::before {
  content: '';
  position: absolute;
  top: 14px;
  left: 7px;
  width: 7px;
  height: 4px;
  background: url(./images/triangle-icon.png) no-repeat center;
  background-size: contain;
}

.banner header .actions .p-message {
  float: left;
  width: 100px;
  margin-right: 8px;
  background: #70757f;
  border-radius: 2px;
  text-align: center;
  line-height: 32px;
  color: #ffffff;
  font-family: PingFang SC;
  font-style: normal;
  font-weight: 500;
  font-size: 14px;
}

.banner header .actions .m-more {
  position: relative;
  float: left;
  width: 30px;
  height: 32px;
  background: #70757f;
  border-radius: 2px;
}

.banner header .actions .m-more::before {
  content: '';
  position: absolute;
  top: 10px;
  left: 8px;
  width: 14px;
  height: 11px;
  background: url(./images/list-icon.png) no-repeat center;
  background-size: contain;
}

.banner nav {
  box-sizing: border-box;
  background: #ffffff;
  box-shadow: 0px 1px 1px rgba(0, 0, 0, 0.15);
}

.banner nav .container {
  width: 332px;
  margin: 0 auto;
}

.banner nav .container span {
  box-sizing: border-box;
  float: left;
  height: 40px;
  font-family: PingFang SC;
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  line-height: 14px;
  color: #000000;
}

.banner nav .container span.left {
  line-height: 40px;
  font-weight: 600;
  border-bottom: 2px solid #f7691d;
}

.banner nav .container span.right {
  float: right;
  line-height: 40px;
}

```