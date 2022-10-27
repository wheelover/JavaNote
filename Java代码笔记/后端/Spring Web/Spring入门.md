#Spring入门
依赖注入(DI)是Spring最核心的技术

##Maven入门
Maven是一个项目管理和构建自动化工具，提供了一个命令行工具可以把工程Java支持的格式（比如jar）支持部署到中央仓库里，其他人添加依赖就可以快捷运用别人的代码
<img src="https://style.youkeda.com/img/ham/course/j4/mvn.svg" alt="image"/>

###Maven使用惯例由于配置的原则
目录 | 目的 
:-- | :-- 
${basedir} | 存放pom.xml和所有子母录 
${basedir}/pom.xml | Maven的项目配置文件
${basedir}/src/main/java | 项目的Java源代码
${basedir}/src/main/resources | 项目多的资源，比如说property文件
${basedir}/src/test/java | 项目的测试类，比如说JUnit代码
${basedir}/src/test/resources | 测试使用的资源

${basedir}代表的是Java工程的根路径，编译后的classes会放在${basedir}/target/classes下面，JAR文件会放在${basedir}/target下面

###Maven命令
1. mvn clean compile：编译命令，扫描Java文件并完成编译工作，在根目录下生成target/classes
2. mvn clean package：编译并打包命令，这个命令是compile和package的集合，把Java文件和资源打包成一个jar
3. mvn clean install：这个是compile，package，install的集合，放到本地的Maven仓库目录这个目录是${user_home}/.m2
4. mvn compile exec:java -Dexec.mainClass=${main}：compile执行后，执行运行Java的命令，比如想执行com.youkeda.Test类，“mvn compile exec:java -Dexec.mainClass=com.youkeda.Test”

<img src="https://style.youkeda.com/img/ham/course/j4/Maven%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image"/>
配置文件：XML格式，它的文件名一定是pom.xml

<img src="https://style.youkeda.com/img/ham/course/j4/pomxml.svg" alt="image"/>
1. Maven坐标
2. Maven工程属性
3. Maven依赖
4. Maven插件

Maven坐标
```
<groupId>com.youkeda.course</groupId>
<artifactId>app</artifactId>
<packaging>jar</packaging>
<version>1.0-SNAPSHOT</version>
```
坐标决定了这个Maven工程部署后存在Maven仓库的文件位置

* SANPSHOT翻译的意思是快照，表示程序还处于不稳定的阶段
* RELEASE表示稳定，正式发布时就把version改为RELEASE，不用特意加上

三位版本号(如iPhone11搭配的操作系统的版本是iOS 13.1.2)
*  第一位代表的是主版本号
* 第二位代表的是新增功能
* 第三位代表的是bugfix后的版本
两位版本，就是没有第一位的主版本号

Maven属性配置
```
 <properties>
    <java.version>1.8</java.version>
    <maven.compiler.source>${java.version}</maven.compiler.source>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.target>${java.version}</maven.compiler.target>
</properties>
```

依赖管理dependencies
```
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.62</version>
    </dependency>
    <dependency> 
        <groupId>com.squareup.okhttp3</groupId>
        <artifactId>okhttp</artifactId>
        <version>4.2.2</version>
    </dependency>
</dependencies>
```
我们会把别人写的代码称为三方库，自己,团队写的称为二方库
1.1中央仓库https://developer.aliyun.com/mvn/search (阿里云镜像服务器)
1.2间接依赖，如果一个remote工程依赖了okhttp库，而当前工程local依赖了remote工程，local工程也会自动依赖了okhttp

插件体系plugins
```
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
        </plugin>
    </plugins>
</build>
```
maven-compiler-plugin插件用于执行maven compile，maven插件其实也是存放在中央仓库的坐标

##Hello Spring
Spring5的坐标如下
```
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
  <version>5.2.1.RELEASE</version>
</dependency>
```
Spring强调的是面向接口编程，大多情况下Spring都会有接口和实现类

例子：
```
package com.youkeda;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.ComponentScan;
import com.youkeda.service.MessageService;

/**
 * Application
 */
@ComponentScan
public class Application {

    public static void main(String[] args) {

        ApplicationContext context = new AnnotationConfigApplicationContext(Application.class);
        MessageService msgService = context.getBean(MessageService.class);
        System.out.println(msgService.getMessage());

    }
}
```
知识点：
* 注解
* Spring Bean
* Spring扫描
* Spring声明周期

面向接口
<img src="https://style.youkeda.com/img/ham/course/j4/springbeanvs.svg" alt="image"/>
对比：在Spring中，调用MessageService就可以直接从上下文获取，而不用关心它的实现类，如何实例化实现类

































