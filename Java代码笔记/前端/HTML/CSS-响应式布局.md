#响应式布局
就是一个网站能够兼容多个终端，而不是为每个终端做一个特定的版本
通过规定特定的宽度范围使用特定的布局来实现在不同的设备上应用不同的布局
这里我们就要用到CSS3中的媒体查询@media了

##媒体查询：
基础用法：
<img src="https://document.youkeda.com/P3-2-HTML-CSS/1.7/18.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image text"/>
screen:告知设备在打印页面是使用衬线字体，在屏幕上显示时用无衬线字体。如果不考虑用户去打印，可以直接去掉“screen and”

条件：最大宽度（max-width）
当浏览器的宽度小于等于900px时，隐藏左侧大图的样式代码（考虑样式层叠性）

逻辑操作符and
<img src="https://document.youkeda.com/P3-2-HTML-CSS/1.7/21.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image text"/>

##断点
我们用两个点（900px和1080px）把浏览器宽度分成了三段，在这三段中页面的左侧大图有不同的表现，这两个（900px和1080px），就是我们所说的断点
总结：
1. 媒体查询用max-width表示条件的时候，大的断点放在上面
2. 反过来，用min-width表示条件的时候，小的断点放在上面
<img src="https://document.youkeda.com/P3-2-HTML-CSS/1.7/17.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image text"/>