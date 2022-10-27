#CSS-高级美化

##单文本溢出省略
正常情况下，文本内容超出了所给的宽度的范围，文字会自动换行。但我们并不希望他换行，这时候我们就需要进行单行文本溢出省略

##步骤：
###1. 强制不换行
HTML中推荐使用“white-space:nowrap”实现不换行（超出的内容课横向滚动查看）

###2. 元素内容溢出overflow
目标是超出部分不显示，并且出现省略号，这里我们设置超出隐藏，用到属性overflow
该属性有5个有效值
visible：默认值，从父元素继承overflow的属性
hidden：内容会被修剪，超出内容不可见
inherit：内容不会被修剪，会呈现在元素框外
scroll：内容会被修剪，浏览器会显示滚动条以便查看超出的内容
auto：由浏览器定夺，如果内容被修剪，就会显示滚动条

###3. 文本溢出省略text-over-flow
该属性有两个值：clip，ellipsis
clip：默认值，表示在内容区域的极限处阶段文本，把文本一刀切
ellipsis：用一个省略号表示被阶段的文本
代码：
```
/* 强制不换行 */
white-space: nowrap;
/* 隐藏超出部分 */
overflow: hidden;
/* 用省略号代替剩余内容 */
text-overflow: ellipsis;
```
在flex容器中做文本溢出省略必须在文本元素的父元素上设置overflow: hidden

##多行文本超出省略
WebKit内核浏览器支持多行文本溢出省略
代码
```
/* 隐藏超出部分 */
overflow : hidden;
/* 文本超出就用省略号 */
text-overflow: ellipsis;
/* 把对象作为弹性伸缩盒子模型显示 */
display: -webkit-box;
/* WebKit内核的浏览器的私有属性，设置文本超出2行就用省略号 */
-webkit-line-clamp: 2;
/* WebKit内核的浏览器的私有属性，设置或检索伸缩盒对象的子元素的排列方式 */
-webkit-box-orient: vertical;
```
先把“white-space：nowrap”去掉








