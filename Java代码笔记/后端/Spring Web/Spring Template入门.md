#Spring Template入门
##Thymleaf
支持动态页面开发，这是一个摸板框架
<img src="https://style.youkeda.com/img/ham/course/j4/template1.svg" alt="image"/>
一句话概括就是 *数据+摸板+引擎*渲染出真实的页面

依赖添加
```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```
要运用Thymleaf只需要添加一个Model对象
```
import org.springframework.ui.Model;
```

例子：
```
@Controller
public class SongListControl {

  @Autowired
  private SongListService songListService;

  @RequestMapping("/songlist")
  public String index(@RequestParam("id")String id,Model model){

    SongList songList = songListService.get(id);
    //传递歌单对象到模板当中
    //第一个 songList 是模板中使用的变量名
    // 第二个 songList 是当前的对象实例
    model.addAttribute("songList",songList);

    return "songList";
  }
}

html:
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <link rel="stylesheet" href="/css/songList.css" />
    <title>豆瓣歌单</title>
  </head>
  <body>
    <h1 th:text="${songList.name}"></h1>
  </body>
</html>
```
模板文件要放在工程的src/main/resources/templates(固定存放位置)
放在src/main/resources/static目录下的就不是模板，是静态文件
xmlns:th="http://www.thymeleaf.org" 的作用是在写代码的时候让软件能识别thymeleaf
return "songList";其实会去查找src/main/resources/templates/songList.htm，系统会自动匹配后缀名

###Thymeleaf变量
语句：model.addAttribute("msg",str);
* 第一个参数设置的就是上下文变量名(变量名是可以随便定义)
* 第二参数设置的是变量值(可以是任意的对象)
```
import org.springframework.ui.Model;

@Controller
public class DemoControl {

  @RequestMapping("/demo")
  public String index(Model model){
    
    SongList songList = new SongList();
    songList.setId("0001");
    songList.setName("爱你一万年");

    model.addAttribute("sl",songList);
    return "demo";
  }
}

html:
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8" />
  </head>
  <body>
    <span th:text="${sl.id}"></span>
    <span th:text="${sl.name}"></span>
  </body>
</html>
```
如果对象包含对象，还是可以用.一直点出来的，前提是这个对象是POJO类


##Thymeleaf循环语句
```
<ul th:each="song : ${songs}">
  <li th:text="${song.name}">歌曲名称</li>
</ul>

html:
  @RequestMapping("/demo")
  public String index(Model model){
    List<Song> songs = new ArrayList<>();

    Song song = new Song();
    song.setId("0001");
    song.setName("朋友");
    songs.add(song);

    song = new Song();
    song.setId("0002");
    song.setName("夜空中最亮的星");
    songs.add(song);

    model.addAttribute("songs",songs);
    return "demo";
  }
```
打印列表的索引值
```
<ul th:each="song,it: ${songs}">
  <li>
    <span th:text="${it.count}"></span>
    <span th:text="${song.name}"></span>
  </li>
</ul>
```
th:each="song,it: ${songs}"，多了个,it这个it是作为可选参数出行，我们可以由此获取更多关于统计的需求
* it.index：从0开始显示行数
* it.count：从1开始显示行数
* it.size：被迭代对象的大小，获取列表长度
* it.current：当前迭代变量，等同于上面的song
* it.even / odd：布尔值，当前循环是否是偶数/奇数（从1开始计算）
* it.last：布尔值，当前循环是否是最后一个

##Thymeleaf表达式
该表达式主要用于两种场景
* 字符串处理
* 数据转化

###字符串处理
我们可以通过+完成字符串的拼接
```
<span th:text="'00:00/'+${totalTime}"></span>
```
要注意'00:00/'，我们使用 ‘ 包住00:00/这个文本的作用在于把他编程Java字符串

字符串拼接优化：可以使用 | 包围字符串，就不用加 ' 了
```
<span th:text="|00:00/${totalTime}|"></span>
```

###数据转化
依赖添加，该库会自动添加一个新的工具类temporals
```
<dependency>
  <groupId>org.thymeleaf.extras</groupId>
  <artifactId>thymeleaf-extras-java8time</artifactId>
  <version>3.0.4.RELEASE</version>
</dependency>
```
工具类：#{工具类}       变量：${变量名}

我们一般使用dates/temporals用于处理日期类型到字符串的转化
date支持的是Date类，temporals支持的是LocalDate和LocalDateTime
显示年月日
```
<p th:text="${#dates.format(dateVar, 'yyyy-MM-dd')}"></p>
<p th:text="${#dates.format(dateVar, 'yyyy年MM月dd日')}"></p>
```
显示年月日时分秒
```
<p th:text="${#dates.format(dateVar, 'yyyy-MM-dd HH:mm:ss')}"></p>
<p th:text="${#dates.format(dateVar, 'yyyy年MM月dd日 HH时mm分ss秒')}"></p>

Java:
  @RequestMapping("/demo")
  public String index(Model model){

    Date dateVar = new Date();

    model.addAttribute("dateVar",dateVar);
    return "demo";
  }
```
如果日期类型是LocalDate/LocalDateTime那么就把#dates换成#temporals
```
  @RequestMapping("/demo")
  public String index(Model model){
    LocalDateTime dateVar = LocalDateTime.now();

    model.addAttribute("dateVar",dateVar);
    return "demo";
  }
```

还有#strings支持字符串的数据处理
* ${#strings.toUpperCase(name)} 把字符串改成全大写
* ${#strings.toLowerCase(name)} 把字符串改成全小写
* ${#strings.arrayJoin(array,',')}  把字符串数组合并成一个字符串，并以 , 连接
* ${#strings.arraySplit(str,',')}   把字符串分割成一个数组 如果abc没有匹配到，执行后会变成["abc"]
* ${#strings.trim(str)}               把字符串去空格，左右空格都会去掉
* ${#strings.length(str)}           得到字符串的长度
* ${#strings.equals(str1,str2)} 比较两个字符串是否相等
* ${#strings.equalsIgnoreCase(str1,str2)} 忽略大小写后比较两个字符串是否相等

###内联表达式
有些时候我们还是喜欢直接把变量写在HTML中
```
<span>Hello [[${msg}]]</span>
```

##Thymeleaf条件语句
if表达式的值是ture的情况下就会执行渲染
```
<span th:if="${user.sex == 'male'}">男</span>
```
还可以用th:unless代表否定条件
```
<span th:unless="${user.sex == 'male'}">女</span>
```

### #strings逻辑判断
isEmrty：检查字符变量是否为空
```
${#strings.isEmpty(name)}
```
数组也可以
```
${#strings.arrayIsEmpty(name)}
```
集合也可以
```
${#strings.listIsEmpty(name)}
```

contains：检查字符变量是否包含片段
```
${#strings.contains(name,'abc')}
```
* ${#strings.containsIgnoreCase(name,'abc')}：大小写忽略，然后判断是否包含
* ${#strings.startsWith(name,'abc')}：判断字符串是否以abc开头
* ${#strings.endsWith(name,'abc')}：判断字符串是否以abc结束


redirect跳转来自动更新跳转到展示页面




































