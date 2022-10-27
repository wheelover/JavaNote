#CSS-预处理器
SASS是一款CSS预编译器，定义了一种新的编程语言，为CSS增加了一些编程特性
SASS可以有变量，函数，控制语句，导入，嵌套等高级功能，类似的还有less，Stylus
SASS的写法比SCSS简单（跟python很像）
SCSS需要使用分好和花括号
SASS使用缩进和换行
在css属性的基础上Sass提供了一些名为SassScript的新功能

#变量
##变量
以美元符号"$"开头，赋值方式和css属性的写法一样
使用变量
```
$width: 10px;
#main {
  width: $width;
}
```
编译成CSS
```
#main {
  width: 10px;
}
```
如果变量类型为字符串，一般不需要加引号，特殊情况（字符串中有“//”,就需要用英文输入法下的单引号或双引号包裹字符串）
```
// 变量需要定义在使用之前
$position: "absolute";
$mainTxtColor: '#333';

// 应用
#main {
  position: $position;
  color: $mainTxtColor;
}
```
除了简单的赋值，SASS中变量还可以定义类似数组的变量
```
$animals: puppy kitty chick;
```
##简单计算
Sass支持在使用变量时进行简单的加减乘除计算，这样就可以很方便的定义宽高比固定的元素，比如
```
$width: 10px;

#main {
  width: $width / 2;
  height: $width * 2;
}

编译成css：
#main {
  width: 5px;
  height: 20px;
}
```
##插值法
“#{}”插值几乎可以在Sass样式表的任何地方使用
```
$name: "mail";
$top-or-bottom: "top";
$left-or-right: "left";

.icon-#{$name} {
  background-image: url("/icons/#{$name}.svg");
  position: absolute;
  #{$top-or-bottom}: 0;
  #{$left-or-right}: 0;
}
```
将#{}替换为冒号后面的内容
可以用插值法插入任何类型的变量，不仅仅是字符串


#嵌套
解决选择器太长的问题，把共同的选择器提出来
```
main .double .item .links {
  text-align: center;
  a {
    margin-right: 20px;
  }
}
```
嵌套规则：
1. 允许一套css样式嵌套进另一套样式中，内层样式将它外层的选择器作为父选择器
<img src="https://document.youkeda.com/P3-2-HTML-CSS/1.6/3.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image text"/>

2. 父选择器&：在嵌套css规则时，有时也需要使用嵌套外层的父选择器，例如：
<img src="https://document.youkeda.com/P3-2-HTML-CSS/1.6/4.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image text">
还有特殊一点的用法：
<img src="https://document.youkeda.com/P3-2-HTML-CSS/1.6/5.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image text"/>


#复用：mixin/include
用混合（mixin/include)来定义可重复使用的样式
##无参数混合
```
@mixin square {
  width: 100px;
  height: 100px;
}

// 应用：
.user-avatar {
  @include square;
}
.admin-avatar {
  @include square;
}
```
这段代码中定义了一个可复用的代码块，并取名为square，通过关键词@include，我们在div.admin-avatar中应用了这个样式
使用方法：“@mixin：定义可复用的样式“ @include：应用可复用的样式

##有参数的混合
###参数无默认值
```
@mixin square($size) {
  width: $size;
  height: $size;
}

// 应用
.avatar {
  @include square(100px);
}
```
这里允许使用者在使用square的时候传入一个值$size
###参数有默认值
```
@mixin square($size: 100px) {
  width: $size;
  height: $size;
}

// 不传参数就会使用默认的值 100px
.avatar {
  @include square;
}

// 传入参数就会使用传入的值 200px
.avatar-200 {
  @include square($size: 200px);
}
```

#@each
@each $var in <list or map>
　　语法简要说明如下。
$var: 它代表了变量的名称。 @each规则将 $var 每个项目在列表中使用 $var 值输出样式。

<list 或 map>: 这是 SassScript 表达式这将返回一个列表或映射。scss

　　scss代码实例：

@each $color in red, green, yellow, blue {
  .p_#{$color} {
    background-color: #{$color};
  }
}
　　编译后的css结果为：

.p_red {
  background-color: red; }

.p_green {
  background-color: green; }

.p_yellow {
  background-color: yellow; }

.p_blue {
  background-color: blue; }





























