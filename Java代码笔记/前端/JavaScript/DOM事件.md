#DOM事件
通过addEventListener(eventName, callback)绑定eventName事件
打印DOM事件对象
```
const h1 = document.querySelector("h1");
h1.addEventListener("click", function (e) {
  console.log(e);
});
```
搜索常用事件，在MDN中搜索”事件参考“

焦点事件：
focus：表单组件（Input，Textarea，etc）  获取焦点事件
blur：表单组件（Input，Textarea，etc）失去焦点事件

鼠标事件：
click：点击事件
dblclick：双击事件
mousedown：在元素上按下任意鼠标按键
mouseleave：指针溢出元素范围外(不冒泡)
mousemove：指针在元素内移动时持续触发
mouseout：指针移出元素，或者移动到他的子元素上
mouseup：在元素上释放任意鼠标按键

键盘事件：
keydown：键盘按下事件
keyup：键盘释放事件

视图事件：
srcoll：文档滚动事件
resize：窗口放缩事件

资源:
load：资源加载成功事件

##点击事件
点赞：
```
// 默认是未点击喜欢
let hasLike = false;

const likeBtn = document.querySelector(".like-btn");
const likeBtnRight = likeBtn.querySelector(".right");
likeBtn.addEventListener("click", function () {
  // 点击事件
  hasLike = !hasLike;
  if (hasLike) {
    likeBtn.classList.add("has-like");
    likeBtnRight.innerHTML = parseInt(likeBtnRight.innerText.trim()) + 1;
  } else {
    likeBtn.classList.remove("has-like");
    likeBtnRight.innerHTML = parseInt(likeBtnRight.innerText.trim()) - 1;
  }
});
```
展开：
```
let showMore = false;
const moreBtn = document.querySelector(".workspace-item-more .select");
const morePanel = document.querySelector(".workspace-item-more .options");
moreBtn.addEventListener("click", function () {
  showMore = !showMore;

  // 控制morePanel的显示和隐藏
  if (showMore) {
    morePanel.style.display = "block";
  } else {
    morePanel.style.display = "none";
  }
});
```

##冒泡
点击事件先出发Like的click事件，再次出发workspace的click事件，就是事件冒泡
1.点击事件出发其中选中的节点的监听事件
2.冒泡找到父亲节点，出发父亲节点的监听事件
3.依次冒泡知道htnl根元素为止
阻止冒泡：e.stopPrapagation();

##捕获
捕获是从根HTML绩点开始依次移动到当前元素
想要捕获就要传递第三个参数
```
dom.addEventListener('click', function() {}, true);
```

##委托
是冒泡事件的一种应用，如果想要在大量子元素中单击任何一个都可以运行一段代码，可以将监听器设置在其父节点上，并让子节点上发生的事件冒泡到父节点上，而不是每个子节点单独设置事件监听器，举个例子
```
监听每个图片的点击事件
const box = document.querySelector('.box');
const imgArr = box.children;

for (let i = 0; i < imgArr.length; i++) {
  imgArr[i].addEventListener('click', function() {
    document.body.style.backgroundImage = `url(${imgArr[i].src})`;
  });
}
```
改：
```
const box = document.querySelector('.box');

box.addEventListener('click', function(e) {
  // 注意box区域比img大，如果点击在空白间隔区域，那么返回的节点将不会是IMG，需要特殊处理一下
  if (e.target.nodeName === 'IMG') {
    document.body.style.backgroundImage = `url(${e.target.src})`;
  }
});
```


##移动事件
1. mousemove：鼠标移动
2. mouseenter/mouseleave：鼠标进入和离开事件，仅仅只作用与当前DOM节点
3. mouseover/mouseout(常用)：鼠标进入和离开，除了作用于当前元素，同时作用于其后代节点
多级导航代码：
```
let data = [
  {
    title: '新建',
    icon: 'https://style.youkeda.com/img/pizza/context-menu/new.png',
    children: [
      {
        title: '文件',
        icon: 'https://style.youkeda.com/img/pizza/context-menu/new_file.png'
      },
      {
        title: '文件夹',
        icon: 'https://style.youkeda.com/img/pizza/context-menu/new_folder.png'
      }
    ]
  },
  {
    title: '导入',
    icon: 'https://style.youkeda.com/img/pizza/context-menu/import.png'
  },
  {
    title: '重命名',
    icon: 'https://style.youkeda.com/img/pizza/context-menu/rename.png'
  },
  {
    title: '删除',
    icon: 'https://style.youkeda.com/img/pizza/context-menu/delete.png'
  }
];

function createItem(item) {
  const li = document.createElement('li');
  li.innerHTML = `
  <div class="left">
    <img
      class="icon"
      src="${item.icon}"
    />
    <span>${item.title}</span>
  </div>
    ${
      item.children && item.children.length > 0
        ? `
    <img
      class="more"
      src="https://style.youkeda.com/img/pizza/context-menu/more.png"
    />`
        : ''
    }`;
  return li;
}
const firstMenuBox = document.querySelector('.first');
const secondMenuBox = document.querySelector('.second');
firstMenuBox.innerHTML = '';
secondMenuBox.innerHTML = '';

for (let i = 0; i < data.length; i++) {
  const li = createItem(data[i]);
  firstMenuBox.appendChild(li);
  li.addEventListener('mouseover', function() {
    if (data[i].children && data[i].children.length > 0) {
      secondMenuBox.innerHTML = '';
      secondMenuBox.style.display = 'block';
      for (let j = 0; j < data[i].children.length; j++) {
        secondMenuBox.appendChild(createItem(data[i].children[j]));
      }
    } else {
      secondMenuBox.style.display = 'none';
    }
  });
}

```


##表单元素事件
焦点事件和失去焦点事件(focus,blur)
给QQ注册页的昵称输入框加入点击事件
```
const nick = document.querySelector('input.nick');
nick.addEventListener('focus', function() {
  console.log('获取焦点');
});

nick.addEventListener('blur', function() {
  console.log('失去焦点');
});
```
内容值变化
两种事件可以监听元素内容变化——input和change
change：当用户提交对元素值的更改时出发，change事件不一定会对元素值的每次更改触发
input：只要value值修改就会触发


##滚动事件
场景：
无尽滚动：每次滚动到底部会加载新的内容，再次滚动就会加载新的内容
动态滚动：知乎头部，页面滚动的时候，头部就会切换成新的头部

事件描述scroll
```
window.addEventListener('scroll', function () {
  console.log(window.scrollY);
});
```

无尽滚动
```
window.addEventListener('scroll', function () {
  // 可以通过clientHeight获取内容高度
  const height = document.body.clientHeight;

  // 通过screen.height获取浏览器的高度
  const screenHeight = window.screen.height;

  // 当距离底部的距离小于500时，触发页面新增内容
  if (height - window.scrollY - screenHeight < 500) {
    console.log('加载新文章内容');
    // 在底部添加10张图片
    const div = document.createElement('div');
    let str = '';
    for (let i = 0; i < 10; i++) {
      str += `
       <img
        class="first"
        alt=""
        src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/1.jpg?x-oss-process=image/resize,h_300"
      />
      `;
    }
    div.innerHTML = str;
    document.body.appendChild(div);
  }
});
```
1. 内容高度：document.body.clientHeight
2. 浏览器高度：window.screen.height
3. 滚动距离：window.srcolly
4. 滚动距离底部距离：内容高度 - 浏览器高度 - 滚动距离

















































