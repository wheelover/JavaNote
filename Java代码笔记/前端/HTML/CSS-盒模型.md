#盒模型
p标签，ul标签，li标签等都是在div标签的基础上进行改造的
div就是一个干净透明的矩形，没有默认的margin，padding，border，content

##Content
div默认没有宽度高度，如果div的父标签就是li标签，那么div就默认宽度就是li标签的宽度
width/height：用来设置矩形的宽高，单位px。还可以有%形式（width: 60%;height: 20%;），这里%是相对于父元素的，首先要确定父元素的尺寸是存在的
background-color：背景颜色

---

背景颜色包含了padding和content

---

##Padding（内边距）
padding区域是给矩形四周添加相同的边距（选content-box时是外边距，选border-box时是内边距）
padding-top/bottom/right/left
上下左右的值一样可以写成padding：20px
上下左右的值不一样，可以写成padding: 20px 30px 20px 10px;

###box-sizing
规定了如何计算一个元素的总宽高，它有两个值：content-box/border-box
border-box计算公式：width = border + padding + 内容的宽度   height = border + padding + 内容高度（内容就是content）
content-box：width=内容的宽度  height = 内容的高度

##border（边框）
border-width：边框的粗细，单位是px
border-color：边框的颜色
border-style：solid(实线)/dashed(虚线)
简便方式：border: 2px solid blue;
分别设置边框：border-top-color
border-top：1px solid black;
border-bottom：none 无边框
border-radius：12px 圆角（圆角的属性也是可以拆开设置的）
box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);阴影
 
##Margin（外边距）
margin-top/right/bottom/left
水平距离一个盒子的有margin为30px，第二个盒子的左margin为20px，那么最后两个盒子之间的水平距离就是30+20=50px
垂直距离取大的那一的margin值
盒子左右居中：margin可以使盒子可以在父盒子中左右居中，但是有一个前提就是必须有宽度(width)
 margin:0 auto;


##display：block/none
块元素性质：
1. 独占一行（独占一行的性质跟其width没关系）
2. 可以设置宽高（将其宽高扩大，其背景颜色的范围也会扩大）
display：block（块元素）/inline（行内元素）
display：none（设置了这个属性，标签就会消失，在网页布局中最常用的就是用none，block来控制元素的显示和隐藏）

##display：inline/inline-block
1. 行内元素不能设置宽高
2. 行内元素可以设置padding（可以扩大背景颜色的范围）注可能会出现重叠现象
3. 行内元素可以设置左右margin，但是不能设置上下margin
inline-block：就是一个可以在同一行显示的块元素（性质与block类似）。
空白折叠（回车是两个盒子之间有间隙）
解决办法：
1. 去除回车：将div标签写在一行
2. 给父元素添加word-spacing属性：word-spacing就是单词和单词之间的距离，将这个距离写成负值，一般设置-20px
3. 给父元素设置font-size：0px


