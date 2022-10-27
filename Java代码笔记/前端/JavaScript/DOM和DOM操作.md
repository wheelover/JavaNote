#DOM和DOM操作
DOM(文档对象模型)，让JavaScript可以来操作HTML和CSS（文档可以将web页面和脚本或变成语言连接起来）
web页面：用HTML和CSS的页面，也称作文档
DOM是一种规范，一种约定，只要遵循无论是JavaScript，还是python，或者Java都可以被连接起来

##DOM映射
像HTNL和XML这种形式的文档都是树状结构，也是对应数据结构中的树
```
<html>
  <head>
    <title>youkeda</title>
  </head>
  <body>
    <div>
      <h1>优课达</h1>
      <p>学的比别人好一点</p>
    </div>
  </body>
</html>
```
图示是一颗倒着的树
<img src="https://style.youkeda.com/img/course/f4/9/1.jpeg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image"/>
DOM树特性：
1. 树根是DOCUMENT，也可以称为整个页面文档
2. 每个HTML标签我们称为DOM节点，英文为Node或Element
3. 每个HTML标签包裹的子标签，在树上称为儿子节点，称为儿子节点
4. P，H1是body的孙子节点
5. 所有P，H1的长辈，我们称为P和H1的祖先节点
6. P，H1是亲兄弟，我们称为兄弟节点


##访问DOM
获取DOCUMENT（获得整个页面的内容）
```
window.document;
```
document内容key为documentElement

选择器查询法querySelector()：获得部分的内容
```
document.querySelector('main .core .subtitle');
```

选择器全量查询
```
document.querySelectorAll('input');
```

迭代查询
```
let subtitle = document.querySelector('main .core .subtitle');
console.log(subtitle.querySelector('a'));
```

getElementById():根据id查询某个节点
getElementByClassName():根据class查询多个节点
getElementByTagName():根据标签名查询多个节点


##DOM属性
```
<!-- HTMLDocument 根文档 -->
<html>
  ……
</html>

<!-- HTMLDivElement DIV类型 -->
<div class="subtitle">
  ……
</div>

<!-- HTMLAnchorElement 超链接类型 -->
<a class="free-bright">免费靓号</a>

<!-- HTMLInputElement Input类型 -->
<input class="password" type="pasworkd" placeholder="请输入密码" />
```
###DOM种类
1. 元素节点
2. 特性节点
3. 文本节点
4. 。。。。。
```
let divDom = document.querySelector('div#test');
console.log(divDom.nodeType, divDom.nodeName, divDom.nodeValue);

// 获取DIV节点的第一个儿子节点，代表 '优课达' 这个字符串
let txtDom = divDom.firstChild;
console.log(txtDom.nodeType, txtDom.nodeName, txtDom.nodeValue);

// 获取DIV节点的id属性
let attDom = divDom.attributes.id;
console.log(attDom.nodeType, attDom.nodeName, attDom.nodeValue);
```
divDom：元素节点    txtDom：文本节点  attdom：特性节点

1. 整个HTML中，无论是标签，纯文本字符串，标签属性都是Element，不同的地方在与nodeType分别为1,2,3
2. HTML标签都是元素节点，可以用nodeName获取标签名
3. 纯文本都是文本节点，可以用nodeValue获取文本内容
4. 标签的每个属性都是特性节点，可以用nodeName获得属性Key，用nodeValue取得属性Value
5. attributes可以获得元素节点的所有属性，得到的结果是一个字典。通过属性Key获取对应的特性节点



###DOM内容
```
<!DOCTYPE html>
<head>
  <meta charset="UTF-8" />
  <title>优课达</title>
</head>
<body>
  <div id="test">
    优课达
    <p>youkeda</p>
    <p>学的比别人好一点</p>
  </div>
  <script src="./index.js"></script>
</body>

js:
let divDom = document.querySelector('div#test');
console.log(divDom.outerHTML, divDom.innerHTML, divDom.innerText);
```

###DOM亲属
```
let divDom = document.querySelector('div#test');
console.log(divDom.firstChild, divDom.lastChild);
console.log('-----');
console.log(divDom.childNodes);
console.log('-----');
console.log(divDom.parentNode);
```

###DOM样式
```
<!DOCTYPE html>
<head>
  <meta charset="UTF-8" />
  <title>优课达</title>
</head>
<body>
  <h1 class="test youkeda" style="color: #FF3300; font-size: 24px;">优课达</h1>
  <script src="./index.js"></script>
</body>

js:
const h1Dom = document.querySelector('h1');
console.log(h1Dom.classList);
console.log(h1Dom.style);
console.log(h1Dom.style.color);
```


###DOM数据属性
HTML利用data-.. 准内于HTML元素中存储额外的信息
```
<!DOCTYPE html>
<head>
  <meta charset="UTF-8" />
  <title>优课达</title>
</head>
<body>
  <article data-parts="3" data-words="1314" data-category="python">
    ...
  </article>
  <script src="./index.js"></script>
</body>
```

数据表示：段落，字数，分类，etc

利用js获取
```
const article = document.querySelector('article');
console.log(article.dataset);
```
<img src="https://style.youkeda.com/img/course/f4/9/16.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image"/>
dataset是一个Map对象，它是data-..的..Key-Vaule集合



DOM操作
```
// 保存当前是否选中的状态
let isSelected = false;

// 获取整个元素的节点
const box = document.querySelector('.box');

// 获取select框节点
const select = document.querySelector('.select');

// 给整个元素添加点击事件【大家可以先忽略这部分】
box.addEventListener('click', function () {
  // 点击以后触发这个函数

  // 修改当前选中状态，取反即可
  isSelected = !isSelected;

  // 如果当前是选中状态、则添加img到select中
  if (isSelected) {
    // 创建一个img标签节点
    const img = document.createElement('img');

    // 设置img的src属性和样式，让其撑满select框
    img.src = 'https://style.youkeda.com/img/sandwich/check.png';
    img.setAttribute('style', 'width: 100%; height: 100%;');

    // 将这个节点添加到select框中
    select.appendChild(img);
  } else {
    // 如果不是选择状态，则清空内部子元素
    select.innerHTML = '';
  }
});
```

创建标签节点：const div = document.createElement('div');
添加纯文本内容：const txt = document.createTextNode('优课达-学的比别人好一点');
```
const div = document.createElement('div');
const txt = document.createTextNode('优课达-学的比别人好一点');
div.appendChild(txt);
document.body.appendChild(div);
```
添加新节点：appendChild(newNode)；
appendChild()是在所有儿子节点之后添加，inserBefore()是在之前添加
insertBefore(newNode, referenceNode)，需要两个参数，一个是新节点，一个是目标节点

```
<!DOCTYPE html>
<head>
  <meta charset="utf-8" />
  <title>优课达</title>
</head>
<body>
  <ul class="root">
    <li class="sars">SARS</li>
  </ul>
  <script src="./index.js"></script>
</body>

js:
function createDisease(txt) {
  const dom = document.createElement('li');
  const domTxt = document.createTextNode(txt);
  dom.appendChild(domTxt);
  return dom;
}

const root = document.querySelector('ul.root');
const sars = document.querySelector('li.sars');

// 创建 H1N1
const H1N1 = createDisease('H1N1');
root.appendChild(H1N1);

// 创建 新型冠状病毒
const nCoV = createDisease('新型冠状病毒');
root.insertBefore(nCoV, sars);
```

设置样式属性
```
img.setAttribute('style', 'width: 100%; height: 100%;');
dom.style.color = 'xxxx';//单独设置某些属性
```
setAttribute()不仅仅可以设置style，所有HTML属性都能设置如，id，src，tyoe，disable，etc

增添或修改class
classList：
div.classList.remove("foo");
div.classList.add("anotherclass");


innerHTML
```
function createDisease(txt) {
  const dom = document.createElement('li');

  // 我们可以直接用innerHTML设置其纯文本
  dom.innerHTML = txt;
  return dom;
}
```

DOM操作(二)
监听Input输入事件，处理区域显示隐藏
```
html:
<body>
  <div>
    <nav>
      <!-- 头部搜索框区域 -->
    </nav>
    <main>
      <!-- 输入非'肺炎'情况 -->
      <section class="login">
        登录查看历史
      </section>
      <!-- 输入'肺炎'情况 -->
      <ul class="search-result">
        <li>
          <i class="search"></i>
          <p><em>肺炎</em>疫情实时动态</p>
          <i class="edit"></i>
        </li>
        ……
      </ul>
    </main>
  </div>
</body>


js:
const input = document.querySelector('input');

// 监听键盘事件
input.addEventListener('keyup', function() {
  // this 是DOM节点，this.value可以获取input内输入的值
  console.log(this.value);
});
```

我们需要利用数组数据，生成多个li标签内容，li标签摸板
```
<li>
  <i class="search"></i>
  <p><em>肺炎</em>疫情实时动态</p>
  <i class="edit"></i>
</li>
```
代码如下
```
let data = [
  '<em>肺炎</em>实时疫情动态',
  '<em>肺炎</em>的症状有哪些症状',
  '<em>肺炎</em>武汉',
  '<em>肺炎</em>症状',
  '<em>肺炎</em>最新',
  '<em>肺炎</em>是怎么引起的',
  '<em>肺炎</em>最新消息',
  '<em>肺炎</em>实时',
  '<em>肺炎</em>症状及表现',
  '<em>肺炎</em>最新情况'
];

function createSearchItem(txt) {
  const item = document.createElement('li');
  item.innerHTML = `<i class="search"></i><p>${txt}</p><i class="edit"></i>`;
  return item;
}

const input = document.querySelector('input');

const login = document.querySelector('.login');
const searchResult = document.querySelector('.search-result');

// 监听键盘事件
input.addEventListener('keyup', function() {
  // this 是DOM节点，this.value可以获取input内输入的值
  if (this.value === '肺炎') {
    // 先把原始内容清空
    searchResult.innerHTML = '';
    for (let i = 0; i < data.length; i++) {
      searchResult.appendChild(createSearchItem(data[i]));
    }

    login.style.display = 'none';
    searchResult.style.display = 'block';
  } else {
    login.style.display = 'block';
    searchResult.style.display = 'none';
  }
});
```

































