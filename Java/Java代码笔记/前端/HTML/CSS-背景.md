#CSS-背景
##背景颜色
linear-gradient：渐变颜色
background：linear-gradient(to right, white, bule)渐变方向，开始颜色，结束颜色
to rigth/to left	向左向右渐变
to top/to bottom      向上/向下渐变
to right bottom / to right top  向右下/向有上渐变
to left bottom / to left top   向左下/左上渐变
xxxdeg  xxx范围（0到360） 更加精确的渐变方向

##背景图片
###图片导入
background-image：url(....);
###图片重复
background-repeat:no-repeat/repeat-x/repeat-y/repeat(默认值)
###图片居中
background-position的值
1. background-position：center
top left 	background-position：top left效果等于background-position-x：left
center left
bottom left
2. x% y%
3. xpx ypx
###图片填充整个容器
background-size：
cover：满足图片长宽较小的一方撑满屏幕
contain：满足图片长宽中较大的一方撑满屏幕
xpx ypx：手动设置宽度和高度
x% y%：手动宽度和高度相对于容器的百分比
###background合并写法
background:[background-color] [background-image] [background-repeat] [background-attachment] [background-position] / [background-size] [background-clip]



