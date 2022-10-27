#CSS伪类

##CSS伪元素
伪元素就是利用css代码在标签内部的前面，或者后面添加一个行内元素，这个行内元素你可以理解为span


##事件伪类
hover：鼠标移动上去li：hover{}
active：鼠标点击时

---

注意hover要在active之前，否则不会生效

---

focus：获取焦点后
一般用于具有焦点的元素，比如input

##列表伪类(列表伪类的适用条件是，同一级别相同元素)
:first-child匹配其父元素中的第一个子元素
:last-child匹配父元素中最后一个子元素
:nth-child()匹配父元素中的第n个子元素


##清除浮动
元素浮动后，其他盒子的一部分会跑到这个元素的下面
所以我们要可以让父元素包住浮动的子元素，即清除浮动
方法：
1. <!-- 添加清除浮动类名 --><div class="father-one clearfix">
2. 添加css代码
```
.clearfix::after{
  content: '';
  display: block;
  clear: both;
}
```


