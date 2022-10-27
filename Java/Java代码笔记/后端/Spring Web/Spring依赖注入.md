#Spring依赖注入

##Annotation(注解)
<img src="https://style.youkeda.com/img/ham/course/j4/service1.svg" alt="image"/>

Target：java.lang.annotation.Target自身也是一个注解，它只有一个数组属性，具体类型配置在java.lang.annotation.ElementType枚举类中
	* ElementType.TYPE：作用于类接口，枚举类
	* Element.FILELD：作用于类的属性上
	* ElementType.METHOD：作用于类的方法上
	* ElementType.PARAMETER：作用于类的参数
	同时作用于类和方法上“@Target({ElementType.TYPE,ElementType.METHOD})”，如果某个Target设定为METHOD，那么只能在方法前面上面引用它

ElementType.TYPE
```
@Service
public class MessageServiceImpl implements MessageService{

    public String getMessage() {
         return "Hello World!";
    }

}
```

ElementType.METHOD
```
public class MessageServiceImpl implements MessageService{

    @ResponseBody
    public String getMessage() {
         return "Hello World!";
    }

}
```

Element.FILELD
```
public class MessageServiceImpl implements MessageService{

    @Autowired
    private WorkspaceService workspaceService;

}
```

ElementType.PARAMETER
```
public class MessageServiceImpl implements MessageService{

    public String getMessage(@RequestParam("msg")String msg) {
         return "Hello "+msg;
    }

}
```

Retention：它用于声明该注解的生命周期
有三个值可以选：1.SOURCE：纯注释作用 2.CLASS：在编译阶段有效的 3.RUNTIME：在运行时有效
```
@Retention(RetentionPolicy.RUNTIME)
```

Docunment：作用是将注解中的元素包含到JavaDoc文档中

@interface：声明当前的Java类型是Annotation，固定语法

Annotation属性：
```
String value() default "";
```
它约定了属性的类型，和属性名称，default代表的是默认值，有了这个属性，我们就可以真是引用一个Annotation
```
import org.springframework.stereotype.Service;

@Service
public class Demo {

}
```
需要import
```
import org.springframework.stereotype.Service;

@Service(value="Demo")
public class Demo {

}
```
value缩写
```
import org.springframework.stereotype.Service;

@Service("Demo")
public class Demo {

}
```

注解类
```
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface RequestParam {

    @AliasFor("name")
    String value() default "";


    @AliasFor("value")
    String name() default "";


    boolean required() default true;

    String defaultValue() default ValueConstants.DEFAULT_NONE;
}
```
@AliasFor("name")是别名的椅子
```
@RequestParam("key")
@RequestParam(value="key")
@RequestParam(name="key")
```
三段代码意思一样

具体代码
```
package com.youkeda.service.impl;

import com.youkeda.service.MessageService;
import org.springframework.stereotype.Service;

@Service
public class MessageServiceImpl implements MessageService{

    public String getMessage() {
         return "Hello World!";
    }

}
```

##Spring Bean
在Spring中，所有Java对象都会通过IoC容器转变为Bean
Annotation类型的IoC容器对应的类是
```
org.springframework.context.annotation.AnnotationConfigApplicationContext
```
启动IoC
```
ApplicationContext context = 
    new AnnotationConfigApplicationContext("fm.douban");
```
这个类的构造函数有两种：
* AnnotationConfigApplicationContext(String....basePackages)根据包名实例化
* AnnotationConfigApplicationContext(Class clazz)根据自定义包扫描行为实例化

Spring Bean的注解有：
@Component：通用的Bean注解
@Service：代表的是Service Bean
@Controller：作用于Web Bean
@Repository：作用于持久化相关Bean
```
package fm.douban;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import fm.douban.service.SongService;
import fm.douban.model.Song;

/**
 * Application
 */
public class Application {

  public static void main(String[] args) {

    ApplicationContext context = new AnnotationConfigApplicationContext("fm.douban");
    // 从容器中获取歌曲服务。
    SongService songService = context.getBean(SongService.class);
    // 获取歌曲
    Song song = songService.get("001");
    System.out.println("得到歌曲：" + song.getName());

  }
}
```
*面向接口编程*

实现类例子：
```
package fm.douban.service.impl;

import fm.douban.model.Song;
import fm.douban.model.Subject;
import fm.douban.service.SongService;
import fm.douban.service.SubjectService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

@Service
public class SubjectServiceImpl implements SubjectService {

    @Autowired
    private SongService service;

    private  static Map<String, Subject> map = new HashMap<>();

    static {
        Subject subject = new Subject();
        subject.setId("s001");
        subject.setName("成都");
        subject.setMusician("赵雷");
        map.put(subject.getId(), subject);
    }

    @Override
    public Subject get(String subjectId) {
        Subject subject = map.get(subjectId);

        List<Song> songs = service.list(subjectId);
        subject.setSongs(songs);
        return subject;
    }
}

```



##Spring Resource(文件系统编程)
classpath：类似虚拟目录，用来读取jar中的文件，需要借助Java IO读取，classpath指定的文件不能解析成File对象，但是可以解析成InputStream
用commons-io这个库，依赖：
```
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.6</version>
</dependency>
```
测试代码
```
public class Test {

  public static void main(String[] args) {
    // 读取 classpath 的内容
    InputStream in = Test.class.getClassLoader().getResourceAsStream("data.json");
    // 使用 commons-io 库读取文本
    try {
      String content = IOUtils.toString(in, "utf-8");
      System.out.println(content);
    } catch (IOException e) {
      // IOUtils.toString 有可能会抛出异常，需要我们捕获一下
      e.printStackTrace();
    }
  }

}
```
从Java运行的类加载器(ClassLoader)实例中查找文件，Test.class指的当前的Test.java编译后的Java class文件


FileService实现类：
```
import fm.douban.service.FileService;
import org.apache.commons.io.IOUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.io.Resource;
import org.springframework.core.io.ResourceLoader;
import org.springframework.stereotype.Service;

import java.io.IOException;
import java.io.InputStream;

@Service
public class FileServiceImpl implements FileService {

    @Autowired
    private ResourceLoader loader;//Resource封装

    @Override
    public String getContent(String name) {
        try {
            InputStream in = loader.getResource(name).getInputStream();
            return IOUtils.toString(in,"utf-8");
        } catch (IOException e) {
           return null;
        }
    }
}
```
Resource可以读取jar内的文件，也可以读取工程目录下的文件，还可以加载远程文件，网页内容抓取
```
FileService fileService = context.getBean(FileService.class);
String content = fileService.getContent("classpath:data/urls.txt");
System.out.println(content);

String content2 = fileService.getContent("file:mywork/readme.md");
System.out.println(content2);

String content2 = fileService.getContent("https://www.zhihu.com/question/34786516/answer/822686390");
System.out.println(content2);
```

##Spring Bean的生命周期(Lifecycle)
<img src = "https://style.youkeda.com/img/ham/course/j4/beaninstance.svg" alt = "image"/>
我们只需要掌握init这个方法即可，注意这个init方法名称可以是任意名称
```
import javax.annotation.PostConstruct;

@Service
public class SubjectServiceImpl implements SubjectService {

  @PostConstruct
  public void init(){
      System.out.println("启动啦");
  }

}  
```
只要在方法上添加@PostConstruct注解，就代表该方法在Spring Bean启动后自动执行
POSTConstruct的完整包路径是：javax.annotation.PostConstruct
把static代码块的代码移动到init里
```
@Service
public class SubjectServiceImpl implements SubjectService {

  @PostConstruct
  public void init(){
      Subject subject = new Subject();
      subject.setId("s001");
      subject.setName("成都");
      subject.setMusician("赵雷");
      
      subjectMap.put(subject.getId(), subject);
  }

}  
```

































