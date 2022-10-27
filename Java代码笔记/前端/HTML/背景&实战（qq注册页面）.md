#背景
```
html:
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <link rel="stylesheet" type="text/css" href="./index.css" />
    <title>每日推荐</title>
  </head>
  <body>
    <div class="box">
      <div class="head">星期二</div>
      <div class="date">17</div>
      <div class="mask"></div>
    </div>
  </body>
</html>

css:
.box {
    /* div.mask 相对于 div.box 进行绝对定位 */
    position: relative;
    width: 140px;
    height: 140px;
    border-radius: 4px;
    overflow: hidden;
  }
  
  .head {
    height: 33px;
    line-height: 33px;
    color: #fed9d9;
    font-size: 14px;
    text-align: center;
    background: linear-gradient(to bottom, #d94747, #b9191a);
  }
  
  .date {
    height: 107px;
    line-height: 107px;
    text-align: center;
    font-size: 94px;
    font-family: Arial, Helvetica, sans-serif;
    font-weight: bold;
    color: #202020;
  }
  
  .mask {
    position: absolute;
    top: 33px;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0.05),
      rgba(0, 0, 0, 0.15) 50%,
      rgba(0, 0, 0, 0.05) 50%,
      rgba(0, 0, 0, 0.15) 100%
    );
  }
  
```

```
html:
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <link rel="stylesheet" type="text/css" href="./index.css" />
    <title>每日推荐</title>
  </head>
  <body>
    <div class="banner">
      <div class="box">
        <div class="head">星期二</div>
        <div class="date">17</div>
        <div class="mask"></div>
      </div>
    </div>
  </body>
</html>

css:
.banner {
    background-image: url(https://document.youkeda.com/P3-1-HTML-CSS/1.9/music-bg.jpg);
    background-size: cover;
    padding-left: 48px;
    padding-top: 30px;
    box-sizing: border-box;
    width: 715px;
    height: 200px;
  }
  
  .box {
    position: relative;
    width: 140px;
    height: 140px;
    border-radius: 4px;
    overflow: hidden;
  }
  
  .head {
    height: 33px;
    line-height: 33px;
    color: #fed9d9;
    font-size: 14px;
    text-align: center;
    background: linear-gradient(to bottom, #d94747, #b9191a);
  }
  
  .date {
    background-color: white;
    height: 107px;
    line-height: 107px;
    text-align: center;
    font-size: 94px;
    font-family: Arial, Helvetica, sans-serif;
    font-weight: bold;
    color: #202020;
  }
  
  .mask {
    position: absolute;
    top: 33px;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0.05),
      rgba(0, 0, 0, 0.15) 50%,
      rgba(0, 0, 0, 0.05) 50%,
      rgba(0, 0, 0, 0.15) 100%
    );
  }
  
```

```
html:
<!DOCTYPE html>
<head>
  <meta charset="UTF-8" />
  <link rel="stylesheet" type="text/css" href="./index.css" />
  <title>微博</title>
</head>
<body>
  <div class="banner">
    <div class="main">
      <div class="info">
        <div class="title">
          #一直单身但颜值超高的人#
        </div>
        <div class="ext">
          <span class="discuss">1.4万讨论</span>
          <span class="read">2443.3万阅读</span>
        </div>
      </div>
    </div>
  </div>
</body>

css:
.banner {
    position: relative;
    width: 600px;
    height: 300px;
    background-image: url(https://document.youkeda.com/P3-1-HTML-CSS/1.9/weibo-banner.jpg);
    background-size: cover;
  }
  
  .info {
    float: left;
  }
  
  .main {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 60px;
    padding-left: 10px;
    color: #fff;
    background: linear-gradient(to bottom, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.9));
  }
  
  .title {
    margin-top: 10px;
    font-size: 18px;
    font-weight: 700;
    line-height: 20px;
  }
  
  .ext {
    margin-top: 4px;
    font-size: 12px;
    line-height: 20px;
  }
  
  .discuss {
    margin-right: 20px;
  }
  
```

```
html:
<!DOCTYPE html>
<head>
  <meta charset="UTF-8" />
  <link rel="stylesheet" type="text/css" href="./index.css" />
  <title>QQ注册</title>
</head>
<body>
  <nav class="nav">
    <a class="qq">
      <img src="https://document.youkeda.com/P3-1-HTML-CSS/1.9/3-qq/qq.png" />
      <span>QQ</span>
    </a>
    <ul class="right">
      <li class="bright">
        <img
          src="https://document.youkeda.com/P3-1-HTML-CSS/1.9/3-qq/bright.png"
          alt="QQ靓号"
        />
      </li>
      <li class="language">
        <span>简体中文</span>
        <img
          class="arrow"
          src="https://document.youkeda.com/P3-1-HTML-CSS/1.9/3-qq/arrow-down.png"
        />
      </li>
      <li class="contact">意见反馈</li>
    </ul>
  </nav>
  <main class="main">
    <div class="bg"></div>
    <div class="content">
      <div class="core">
        <h1>欢迎注册QQ</h1>
        <div class="subtitle">
          <h2>每一天，乐在沟通。</h2>
          <a class="free-bright">免费靓号</a>
        </div>

        <form action="">
          <input type="text" placeholder="昵称" />
          <input class="password" type="password" placeholder="密码" />
          <div class="mobile">
            <select>
              <option>+86</option>
              <option>+852</option>
            </select>
            <input type="text" placeholder="手机号码" />
          </div>
          <p class="mobile-tip">可通过该手机号找回密码</p>
          <button class="submit">立即注册</button>
          <div class="agreement">
            <input type="checkbox" />
            <label>我已阅读并同意相关服务条款和隐私政策</label>
          </div>
        </form>
      </div>
      <footer>Copyright © 1998-2019Tencent All Rights Reserved</footer>
    </div>
  </main>
</body>

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

/* 浏览器都有自己的默认样式，以上部分为清除浏览器默认样式 */

.nav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  padding: 0 20px;
}

a.qq {
  margin-left: 10px;
  margin-top: 20px;
  float: left;
  cursor: pointer;
  font-size: 0;
}

a.qq > img {
  width: 40px;
  height: 40px;
  vertical-align: middle;
}

a.qq > span {
  vertical-align: middle;
  font-size: 36px;
  line-height: 43px;
  margin-left: 6px;
}

ul.right {
  float: right;
}

ul.right > li {
  float: left;
  margin-top: 20px;
  margin-right: 40px;

  font-size: 16px;
  line-height: 24px;

  cursor: pointer;
}

.bright {
  width: 95px;
  height: 34px;
}

.bright > img {
  width: 100%;
  height: 100%;
}

ul.right > li.language {
  font-size: 0;
  margin-top: 30px;
}

ul.right > li.language span {
  font-size: 16px;
  vertical-align: middle;
}

ul.right > li.language .arrow {
  vertical-align: middle;
  width: 12px;
  height: 8px;
}

ul.right > li.contact {
  margin-top: 30px;
}

/*上面是QQ注册页头部CSS*/

.main {
  height: 100%;
  overflow: hidden;
}

.bg {
  float: left;
  width: 480px;
  height: 100%;
  background: url(http://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.11/bg.png);
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

.content {
  position: relative;
  box-sizing: border-box;
  width: calc(100% - 480px);
  margin-left: 480px;
  height: 100%;
  overflow: hidden;
}

.core {
  width: 480px;
  height: 571px;
  position: absolute;
  left: 50%;
  top: 50%;
  margin-left: -240px;
  margin-top: -285.5px;
}

h1 {
  font-size: 44px;
  line-height: 62px;
  font-weight: normal;
}

.subtitle {
  margin-top: 13px;
  height: 34px;
}

h2 {
  font-size: 27px;
  line-height: 34px;
  font-weight: normal;
  float: left;
}

.free-bright {
  float: right;
  font-size: 23px;
  line-height: 34px;
  color: #359eff;
  cursor: pointer;
}

form {
  margin-top: 63px;
}

select,
input {
  box-sizing: border-box;
  width: 480px;
  height: 52px;
  border: 1px solid #aaaaaa;
  border-radius: 4px;
  padding: 15px 20px;
  font-size: 19px;
}

.mobile,
input.password {
  margin-top: 32px;
}

.mobile > select {
  width: 154px;
  background-color: white;
  outline: none;
}

.mobile > input {
  width: 307px;
  float: right;
}
::placeholder {
  color: #aaaaaa;
  font-size: 19px;
  font-weight: normal;
}

.mobile-tip {
  margin-top: 10px;
  font-size: 13px;
  line-height: 14px;
  color: #999;
}

.submit {
  margin-top: 38px;
  width: 480px;
  box-sizing: border-box;
  height: 60px;
  background-color: #3487ff;
  border: 1px solid #3083ff;
  box-sizing: border-box;
  box-shadow: 0px 5px 8px rgba(24, 95, 255, 0.1);
  border-radius: 4px;
  outline: none;

  font-weight: 200;
  font-size: 24px;
  line-height: 60px;
  color: #ffffff;
  cursor: pointer;
}

.agreement {
  margin-top: 32px;
  font-size: 13px;
  line-height: 30px;
  color: #aaaaaa;
}

.agreement > input {
  width: 18px;
  height: 18px;
  vertical-align: middle;
  padding: 0;
  margin: 0;
}

.agreement > label {
  vertical-align: middle;
}

footer {
  position: absolute;
  width: 344px;
  bottom: 26px;
  left: 50%;
  margin-left: -172px;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  color: #bbbbbb;
}

```