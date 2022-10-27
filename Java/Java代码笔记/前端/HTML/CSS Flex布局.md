#CSS Flex布局
##Flex
在父元素中添加flex，把原本从上到下排列的块状元素变成水平排列

```
display: flex
```
注意：设为Flex布局以后，子元素的float，clear和vertical-align属性将失效
采用Flex布局的元素，称为Flex容器（container），它的所有子元素称为Flex项目（item）

##justify-content和align-items
flex布局中控制水平方向分布的属性justify-content(justify-content是容器属性，设置在容器上)
1. 调整水平方向的分布justify-content：该属性定义了项目在水平方向上的对齐方式
该属性有六个有效值
```
justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
```
<img src="https://document.youkeda.com/P3-2-HTML-CSS/1.4/9.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image text"  />

2. 调整垂直方向上的分布align-items(是容器属性，设置在容器上)
该属性定义项目在垂直方向上如何对齐
该属性有五个有效值
```
align-items: flex-start | flex-end | center | baseline | stretch;
```
![image text](https://document.youkeda.com/P3-2-HTML-CSS/1.4/10.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

##flex-wrap
在flex的布局下，项目都排在水平方向上，并且是单行排列，不会换行
想让项目换行可以用flex-wrap属性
该属性有三个值
<img src="https://document.youkeda.com/P3-2-HTML-CSS/1.4/19.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image text" />

##flex:none和flex:1
这里用到项目属性，控制项目不放大，不压缩
给项目设置flex:none
```
html:
<div class="container none">
<div class="item qq">
</div>
css:
.none .item{
	flex:none;
}
```
项目自动填充满剩余空间flex:1
如果一行有剩余的情况下，项目扩大，撑满水平方向，我们应该给所有项目设置flex:1，这样所有项目的宽度都相同，我们可以用这点来平分一行空间
##flex:none和flex:1实际应用
###两栏自适应布局
指的是一栏固定宽度，另一栏随浏览器的变化而自动调节自己的宽度
```
html:
<div class="flex-auto">
  <div class="static"></div>
  <div class="flexible"></div>
</div>
css:
.flex-auto {
  display: flex;
}
.flex-auto .static {
  width: 100px;
  flex: none;
}
.flex-auto .flexible {
  flex: 1;
}
```
###指定项目放大
如果一行有剩余的情况下，我们可以任意指定某个项目放大撑满剩余空间，只要给指定项目设置flex:1


##flex-direction
垂直方向上的“三栏自适应布局”，这样布局的思路是很明确的，我们值需要改变横向的三栏自适应布局中的三栏的排列方向就可以了。
当我们设置flex-direction：column时横向三栏自适应布局就会变成纵向的


---

#主轴和交叉轴
主轴：水平，从左到右
交叉轴：垂直。从上到下
主轴和交叉轴是相互垂直的关系。交叉轴的方向会随着主轴的方向改变而改变，两轴始终保持垂直的关系

---


该属性有四个有效值
<img src="https://document.youkeda.com/P3-2-HTML-CSS/1.4/8.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image text"/>

设置了flex-direction后justify-content和align-itens控制的方向也随之变成相反



















