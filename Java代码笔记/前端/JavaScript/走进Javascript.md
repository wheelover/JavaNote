#走进Javascript
* 核心：ECMScript：语法，类型，语句，关键句，保留字，操作符，对象
* 文档对象模型（DOM）：DOM遍历和范围，DOM事件，DOM样式
* 浏览器对象模型（BOM）：弹出新窗口，移动，缩放，关闭浏览器窗口的功能


JavaScript书写位置：内部/外部
内部：
```
// script标签嵌入JavaScript代码
<script>
    // JavaScript代码
    let name = "Bob";
    function(){
        console.log("我的名字叫："+name);
    }
</script>
```
script标签的位置要在body的内部，并保证是在末尾
外部：
引入方式：<script src='index.js'></script>
其位置与内部书写位置相同


##字符串
对字母数字，单个字母，中文加上引号就是字符串
数字没加引号是数字，加了引号之后就是字符串

##模板字符串
反引号(``)和占位符${expression}
反引号的作用是将字符串和变量包裹起来，占位符的作用是在字符串中插入变量
```
let firstName = "胡";
let lastName = "雪岩";

let say = `大家好，我姓${firstName}，名${lastName}`;

console.log(say);
```

转义符(\)
想在双引号里面写双引号，在前后双引号添加一个转义符

模板字符串，要换行直接回车就好了

在字符串中使用表达式
```
let number1 = 20;
let number2 = 10;
console.log(`两个数的和是：${number1 + number2} 
两个数的差是：${number1 - number2} 。`);
```

摸板字符串中使用三元表达式
```
let str = `这里是${true ? "江苏" : "浙江"}-${true ? "南京" : "常州"}`;

console.log(str); // 这里是江苏-南京
```



使用场景一：
```
// 定义屏幕的宽度，当然这个宽度是根据window的api去获取的
let screen = 760;

// 判断屏幕是大屏还是小屏，这里我们认为大于760px的就是大屏
function isLargeScreen() {
  return screen > 800;
}

// 定义元素的排列方式，大屏row排列，小屏column排列
// 具体什么排列方式，是根据屏幕大小决定的
let item = {
  isCollapsed: screen > 800
};

// 这里我们就要根据上面的信息来动态的获取类名（多个）
const classes = `header ${
  isLargeScreen() ? "" : `icon-${item.isCollapsed ? "column" : "row"}`
}`;

console.log(classes);
```



使用场景二：
```
let htmlCode = `
    <img src='' />
    ${
      true
        ? `<img src='https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1906469856,4113625838&fm=26&gp=0.jpg' />`
        : `<img src='' />`
    }
`;
console.log(htmlCode);
// <img src='' />
//    <img src='https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1906469856,4113625838&fm=26&gp=0.jpg' />
```
注意html代码作为条件后要输出的内容，要用反引号括起来












