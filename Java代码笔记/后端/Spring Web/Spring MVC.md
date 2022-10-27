#Spring MVC
##Spring Boot
降低了Java Web工程的创建和运行和部署难度，它更是一个方案

##Spring Controller入门
<img src="https://style.youkeda.com/img/ham/course/j4/springmvc1.svg" alt="image"/>
三个技术核心：
* Bean的配置：Controller注解运用
* 网络资源的加载：加载网页
* 网址路由的配置：RequestMapping注解的运用

###Controller注解
其本身也是以一个Spring Bean，在类上提供以=一个@Controller注解就可以了
```
import org.springframework.stereotype.Controller;

@Controller
public class HelloControl {


}
```

###加载网页
在Spring Boot应用中，一般把网页放在src/main/resources/static目录下
<img src="https://style.youkeda.com/img/ham/course/j4/hellohtml.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10"  alt="image"/>
controller中，会自动加载static下的html内容
```
import org.springframework.stereotype.Controller;

@Controller
public class HelloControl {

    public String say(){
        return "hello.html";
    }

}
```
* 定义返回类型是String
* return的是html文件路径
resouces属于classpath类型，Spring Boot自动帮我们做了加载，所以我们只需要写
hello.html，文件路径不需要加static
hello.html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>Hello Spring</h1>
</body>
</html>
```

###static子目录
如果文件存在src/main/resources/static/html/hello.html
<img src="https://style.youkeda.com/img/ham/course/j4/hello.html2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image"/>

```
import org.springframework.stereotype.Controller;

@Controller
public class HelloControl {

    public String say(){
        return "html/hello.html";
    }

}
```

###RequestMapping注解
对于Web服务器来说，必须要实现的一个能力就是解析URL，并提供资源内容给调用者
只需要在需要提供Web访问的方法上添加一个@RequestMapping
```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloControl {

    @RequestMapping("/hello")//这里加了斜杠
    public String say(){
        return "html/hello.html";
    }

}

运行代码：
package fm.douban.app;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class AppApplication {

	public static void main(String[] args) {
		SpringApplication.run(AppApplication.class, args);
	}

}

```

###GET Request
get请求参数的解析
比如https://www.baidu.com/s?wd=test
<img src="https://style.youkeda.com/img/ham/course/j4/url.svg" alt= "image"/>

歌单URL
```
https://域名/songlist?id=xxxx
```
在电脑本地开发
```
http://localhost:8080/songlist?id=xxxx
```

###定义参数
@RequestParam("")
```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

@Controller
public class SongListControl {

    @RequestMapping("/songlist")
    public String index( @RequestParam("id") String id){
        return "html/songList.html";
    }

}
```

```
url: https://域名/songlist?listId=xxxx

注解的参数要与URL的param key一样
@RequestMapping("/songlist")
public String index( @RequestParam("listId") String id){
    return "html/songList.html";
}
```

访问正确返回歌单页面，否则返回404错误页面
```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

@Controller
public class SongListControl {

    @RequestMapping("/songlist")
    public String index( @RequestParam("id") String id){
        if("38672180".equals(id)){
            return "html/songList.html";
        }else{
            return "html/404.html";
        }
    }
}
```

获取多个参数
```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

@Controller
public class SongListControl {

  @RequestMapping("/songlist")
  public String index(@RequestParam("id") String id,  @RequestParam("pageNum") int pageNum){
     return "html/songList.html";
  }
}
```

@GetMapping用来明确制定method
```
import org.springframework.web.bind.annotation.*;


@GetMapping("/songlist")
public String index(@RequestParam("id") String id,@RequestParam("pageNum") int pageNum){
  return "html/songList.html";
}
```

非必须传递参数]
就是：@RequestParam(name="pageNum",required = false) int pageNum
```
@GetMapping("/songlist")
public String index(@RequestParam(name="pageNum",required = false) int pageNum,@RequestParam("id") String id){
  return "html/songList.html";
}
```
方法定义参数的顺序和URL的参数顺序无所谓

###输出JSON数据
添加@ResponseBody注解
```
@GetMapping("/api/foos")
@ResponseBody
public String getFoos(@RequestParam("id") String id) {
  return "ID: " + id;
}
```















































