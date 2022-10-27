#Spring Boot入门

##Spring Boot ComponentScan
加了@SpringBootApplication注解的类就是启动类，是整个系统的启动入口
本演示中fm.douban.app.AppApplication类是启动类，而Spring Boot框架就会默认扫描fm.douban.app包及其所有子包，但fm.douban.service，fm.douban.service.impl，不是fm.douban.app的子包，也不会自动实例化Bean

为启动类的注解@SpringBootApplication加一个参数，告知系统需要额外扫描的包
```
@SpringBootApplication(scanBasePackages={"fm.douban.app", "fm.douban.service"})
public class AppApplication {
  public static void main(String[] args) {
    SpringApplication.run(AppApplication.class, args);
  }
}
```

另一种写法
```
@ComponentScan({"fm.service", "fm.app"})
public class SpringConfiguration {
  ... ...
}
```

使用@RestController的类，所有方法都不会渲染Thymeleaf页面，而是都返回数据。等同于@Controller的类的方法上添加@ResponseBody注解


##Spring Boot Logger运用
运用日志系统来记录信息，检查程序项目是否成功运行

###步骤：
1. 配置：
修改Spring Boot系统的标准配置文件application.properties(在项目的src/main/resources/目录下)，增加日志级别设置
```
logging.level.root=info
```
表示所有日志(root)都为info级别
我们也可以为不同的包定义不同的级别
```
logging.level.fm.douban.app=info
```

| 优先级 | 级别 | 含义和作用 |
| --------   | -----:   | :----: |
| 最高        | ERROR      |  错误信息日志     |
| 高          | WARN      |   展示不出错但高风险的警告信息日志    |
| 中        | INFO      |   一般的提示语，普通数据等不紧要的信息日志    |
| 低         | DEBUG     | 进开发阶段需要关注的调试信息日志 |

logging.level.root=error意味着不输出更低优先级的WARN，INFO，DEBUG，只输出ERROR日志，接下来的以此类推，在开发阶段配置为DEBUG，在项目发布时调整为INFO或更高级别


2. 编码：
```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.RestController;
import javax.annotation.PostConstruct;

@RestController
public class SongListControl {
    private static final Logger LOG = LoggerFactory.getLogger(SongListControl.class);

    @PostConstruct
    public void init(){
        LOG.info("SongListControl 启动啦");
    }
}
```
这里注意方法名(info())与日志级别一一对应   error() , warn() , info() , debug()

日志按级别输出：
值输出当前级别的和更高级别的
定义类变量LOG的语句属于固定写法，只需要在不同的类中，修改getLogger()方法参数为当前的类名即可，修饰为static final的目的是尽量复用，一个类无论多少个是咧只需要一个日志对象



##Spring Boot Properties
固定位置，固定名字的文件，框架会自动加载并解析这个配置文件（在项目的src/main/resources/目录下）
###配置文件格式
application.properties配置文件的格式：配置项名称=配置项值
```
logging.level.root=info
logging.level.fm.douban.app=info
```
注意等号两边不要加空格，要写的紧凑一些
约定：
* 配置项名称能准确表达作用，含义，以点 . 分割单词
* 相同前缀的配置项写在一起
* 不同前缀的配置项之间空一行

###自定义配置项
在application.properties配置文件加入自定义的配置项
```
song.name=God is a girl
```
框架就会自动加载并自动解析整个文件

代码中使用自定义的配置项
```
import org.springframework.beans.factory.annotation.Value;

public class SongListControl {
    @Value("${song.name}")
    private String songName;
}
```
这样项目启动的时候，Spring系统就会自动把application.properties配置文件的song.name的值，赋值给SongListControl对象示例的songName变量

修改端口，配置文件：server.port=













































