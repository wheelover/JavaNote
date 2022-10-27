#BOM浏览器对象模型
在BOM里最重要的对象有5个：window，navigator(浏览器)，history，screen，location
##window
window对象处于对象层次的最顶端，是浏览器中的默认对象，添加到window对象中的方法，属性，其作用域都是全局的
<img src="https://style.youkeda.com/img/course/f4/8/1.jpeg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image"/>


##Location属性
<img src="https://style.youkeda.com/img/course/f4/8/3.jpeg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image"/>

###Location方法：reload()
防止无限快速循环，我们设置一个定时器延迟调用reload
```
setTimeout(function () {
  window.location.reload();
}, 3000);
```

跳转到新地址
```
window.location = 'https://www.youkeda.com';
```

##History
```
// 会话记录
['https://www.youkeda.com', 'https://www.baidu.com'];
```
这里用栈储存记录
两个方法：back(),forward()


##Navigator/Screen
属性userAgent，表示当前浏览器的用户代理
































