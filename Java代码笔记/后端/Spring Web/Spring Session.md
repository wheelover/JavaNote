#Spring Session
<img src="https://style.youkeda.com/img/ham/course/j4/j4-7-1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image"/>
使用Cookie进行读，写操作

##Cookie
###读Cookie
增加一个HttpServletRequest参数，通过request.getCookies()取得cookie数组，然后遍历数组
```java
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;

@RequestMapping("/songlist")
public Map index(HttpServletRequest request) {
  Map returnData = new HashMap();
  returnData.put("result", "this is song list");
  returnData.put("author", songAuthor);

  Cookie[] cookies = request.getCookies();
  returnData.put("cookies", cookies);

  return returnData;
}
```

###使用注解读取cookie
增加@CookieValue("xxxx") String xxxx参数即可，注意使用时要填入正确的cookie名字
```Java
import org.springframework.web.bind.annotation.CookieValue;

@RequestMapping("/songlist")
public Map index(@CookieValue("JSESSIONID") String jSessionId) {
  Map returnData = new HashMap();
  returnData.put("result", "this is song list");
  returnData.put("author", songAuthor);
  returnData.put("JSESSIONID", jSessionId);

  return returnData;
}
```
一定要是正确的名字，否则系统解析就会报错


###写Cookie
为control类的方法增加一个HttpServletResponse参数，调用response.addCookie()方法添加Cookie示例对象即可
```Java
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletResponse;

@RequestMapping("/songlist")
public Map index(HttpServletResponse response) {
  Map returnData = new HashMap();
  returnData.put("result", "this is song list");
  returnData.put("name", songName);

  Cookie cookie = new Cookie("sessionId","CookieTestInfo");
  // 设置的是 cookie 的域名，就是会在哪个域名下生成 cookie 值
  cookie.setDomain("youkeda.com");
  // 是 cookie 的路径，一般就是写到 / ，不会写其他路径的
  cookie.setPath("/");
  // 设置cookie 的最大存活时间，-1 代表随浏览器的有效期，也就是浏览器关闭掉，这个 cookie 就失效了。
  cookie.setMaxAge(-1);
  // 设置是否只能服务器修改，浏览器端不能修改，安全有保障
  cookie.setHttpOnly(false);
  response.addCookie(cookie);

  returnData.put("message", "add cookie successfule");
  return returnData;
}
```
Cookie类的构造函数，第一个参数是coolie名称，第二个参数是cookie值


##Spring Session API
<img src="https://style.youkeda.com/img/ham/course/j4/j4-7-2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image"/>
用户ID，登录状态等重要信息不存在客户端，而是存在服务端，避免安全隐患
使用会话机制是，Cookie作为session id的载体与客户端通信

###读操作
从HttpServletRequest对象中得HttpSession对象，使用的语句是request.getSession()
但不同的是返回结果不是数组，是对象。在attribute属性中用key->value的形式储存多个数据

登录信息类：
因为是在网络上传输，就必须实现序列化接口Serializable
登录信息类需要根据具体的需要设计属性字段
演示：
```Java
import java.io.Serializable;

public class UserLoginInfo implements Serializable {
  private String userId;
  private String userName;
}
```
操作代码：
```Java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@RequestMapping("/songlist")
public Map index(HttpServletRequest request, HttpServletResponse response) {
  Map returnData = new HashMap();
  returnData.put("result", "this is song list");

  // 取得 HttpSession 对象
  HttpSession session = request.getSession();
  // 读取登录信息
  UserLoginInfo userLoginInfo = (UserLoginInfo)session.getAttribute("userLoginInfo");
  if (userLoginInfo == null) {
    // 未登录
    returnData.put("loginInfo", "not login");
  } else {
    // 已登录
    returnData.put("loginInfo", "already login");
  }

  return returnData;
}
```

###写操作
记录登录信息到Session，用setAttribute()方法
演示：（略去了校验用户名和密码的步骤）
```Java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@RequestMapping("/loginmock")
public Map loginMock(HttpServletRequest request, HttpServletResponse response) {
  Map returnData = new HashMap();

  // 假设对比用户名和密码成功
  // 仅演示的登录信息对象
  UserLoginInfo userLoginInfo = new UserLoginInfo();
  userLoginInfo.setUserId("12334445576788");
  userLoginInfo.setUserName("ZhangSan");
  // 取得 HttpSession 对象
  HttpSession session = request.getSession();
  // 写入登录信息
  session.setAttribute("userLoginInfo", userLoginInfo);
  returnData.put("message", "login successfule");

  return returnData;
}
```

Cookie存放在客户端，一般不超过4kb，要特别注意，放太多的内容会导致出错

7.2作业代码
```Java
package fm.douban.app.control;

import fm.douban.model.UserLoginInfo;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.PostConstruct;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.util.HashMap;
import java.util.Map;

@RestController
public class UserControl {
    private static final Logger LOG = LoggerFactory.getLogger(UserControl.class);

    @Value("${loginmock.userName}")
    private String mockedName;

    @Value("${loginmock.password}")
    private String mockedPassword;

    @PostConstruct
    public void init() {
        LOG.info("UserControl 启动啦");
    }

    @GetMapping("/loginmock")
    public Map loginMock(@RequestParam("userName") String userName, @RequestParam("password") String password,
                         HttpServletRequest request, HttpServletResponse response) {

        Map returnData = new HashMap();

        if (mockedName.equals(userName) && mockedPassword.equals(password)) {
            UserLoginInfo userLoginInfo = new UserLoginInfo();
            userLoginInfo.setUserId("123456789abcd");
            userLoginInfo.setUserName(userName);
            // 取得 HttpSession 对象
            HttpSession session = request.getSession();
            // 写入登录信息
            session.setAttribute("userLoginInfo", userLoginInfo);
            returnData.put("result", true);
            returnData.put("message", "login successfule");
        } else {
            returnData.put("result", false);
            returnData.put("message", "userName or password not correct");
        }

        return returnData;
    }

    @RequestMapping("/status")
    public Map status(HttpServletRequest request, HttpServletResponse response) {
        Map returnData = new HashMap();

        // 取得 HttpSession 对象
        HttpSession session = request.getSession();
        // 读取登录信息
        String key = "userLoginInfo";
        UserLoginInfo userLoginInfo = (UserLoginInfo) session.getAttribute(key);
        if (userLoginInfo == null) {
            returnData.put("isLogin", false);
        } else {
            returnData.put("isLogin", true);
        }
        return returnData;
    }
}

```


##Spring Session配置
Cookie作为session id的载体，也可以修改属性
application.properties是SpringBoot的标准配置文件，配置一些简单的属性
SpringBoot也提供了编程式的配置方式，主要用于配置Bean
文件名为sess，java类为SpringHttpSession
```Java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringHttpSessionConfig {
  @Bean
  public TestBean testBean() {
    return new TestBean();
  }
}
```
在类上添加@Configuration注解，就表示这是一个配置类
在方法上添加@Bean注解，表示此方法返回的对象示例注册成Bean

###Session配置
添加依赖
```
<!-- spring session 支持 -->
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-core</artifactId>
</dependency>
```

配置类：
在类上添加@EnableSpringHttpSession，开启session
* CookieSerializer：读写Cookie中的SessionId信息
* MapSessionRepository：Session信息在服务器上的存储仓库
```Java
import org.springframework.session.MapSessionRepository;
import org.springframework.session.config.annotation.web.http.EnableSpringHttpSession;
import org.springframework.session.web.http.CookieSerializer;
import org.springframework.session.web.http.DefaultCookieSerializer;

import java.util.concurrent.ConcurrentHashMap;

@Configuration
@EnableSpringHttpSession
public class SpringHttpSessionConfig {
  @Bean
  public CookieSerializer cookieSerializer() {
    DefaultCookieSerializer serializer = new DefaultCookieSerializer();
    serializer.setCookieName("JSESSIONID");
    // 用正则表达式配置匹配的域名，可以兼容 localhost、127.0.0.1 等各种场景
    serializer.setDomainNamePattern("^.+?\\.(\\w+\\.[a-z]+)$");
    serializer.setCookiePath("/");
    serializer.setUseHttpOnlyCookie(false);
    // 最大生命周期的单位是秒
    serializer.setCookieMaxAge(24 * 60 * 60);
    return serializer;
  }

  // 当前存在内存中
  @Bean
  public MapSessionRepository sessionRepository() {
    return new MapSessionRepository(new ConcurrentHashMap<>());
  }
}
```

##Spring Request拦截器
Spring提供了HandlerInterceptor满足这种场景需求
###步骤：
1. 创建拦截器
2. 实现WebMvcConfigurer

创建拦截器：
拦截器必须实现HandlerInterceptor接口，可在三个点进行拦截
1. Controller方法执行之前。常用拦截点，是否登录的验证就要在preHandle()方法中处理
2. Controller方法执行之后。例如记录日志，统计方法执行时间，就要在postHandle()方法中处理
3. 整个请求完成后，例如统计整个请求的执行时间的时候用，在afterCompletion方法中处理
```Java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

public class InterceptorDemo implements HandlerInterceptor {

  // Controller方法执行之前
  @Override
  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

    // 只有返回true才会继续向下执行，返回false取消当前请求
    return true;
  }

  //Controller方法执行之后
  @Override
  public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
      ModelAndView modelAndView) throws Exception {

  }

  // 整个请求完成后（包括Thymeleaf渲染完毕）
  @Override
  public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

  }
}
```
preHandle()方法中有对应的参数，可以使用Session


实现WebMvcConfigurer：
创建一个类实现WebMvcConfigurer，并实现addInterceptors()方法，用于管理拦截器
为拦截器设置拦截范围，常用`addPathPatterns("/**")`表示拦截所有URL

当然也可以用`excludePathPatterns()`方法排除某些URL
```Java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebAppConfigurerDemo implements WebMvcConfigurer {

  @Override
  public void addInterceptors(InterceptorRegistry registry) {
    // 多个拦截器组成一个拦截器链
    // 仅演示，设置所有 url 都拦截
    registry.addInterceptor(new UserInterceptor()).addPathPatterns("/**");
  }
}
```
通常拦截器会放在一个包(例如interceptor)里。管理的放在(例如config)

HttpServletRequest提供了setAttribute()，getAttribute()方法，可以在当前的这次请求中设置和读取额外的属性数据，以key->value的形式存放
```
request.setAttribute("XXXX", new Integer(1));
```
key必须是字符串，而value可以是任何对象
```
Integer startTime = (Integer) request.getAttribute("XXXX");
```
设置什么对象，取出是就转换成什么对象
request.getAttribute("XXXX")方法的返回值是Object，用()转换成实际的对象

























