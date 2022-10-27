#CSS-美化文档
在CSS中，样式是由属性和值组成的，样式定义HTML元素
引入CSS样式的方法：
1. 行内式：在标签中添加声明，关键字style=""
1. 内部样式：将CSS抽取出来，然后在head标签里用<style>标签包裹
1. 外部样式：创建CSS文件



---

居中需要有剩余空间才能居中

---


##字体样式
字体大小：font-size:36px
字体加粗：font-weight：100px
颜色：color
文字对齐：text-align：center，left，right（居中需要有剩余空间才能居中）
行高：line-high（行高决定了文字行与行之间距离的大小，包含文字的大小和文字上下均分的空白区域）

---

行高的作用：
1. 改变段落行与行之间的距离
2. 使文字上下居中，当行高和矩形的高度一样时，文字在矩形上下居中

---

字间距：letter-spacing
字体：font-family：sans-serif（有的网站会多设置几个字体，font-family: 'Goudy Bookletter 1911', sans-serif, 'Gill Sans Extrabold';）字体之间用英文逗号隔开，字体名称中间有空格时候，要加引号，中文字体名称要用“宋体”