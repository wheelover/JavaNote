#Spring Template进阶

form表单
```
<form action="/book/save" method="POST">
```
action为要跳转的页面地址
input的name属性要和Book类的属性名一致


##Spring Validation
Spring Validation用来处理表单数据的验证 
JSR 380意思是Java规范提案，其实就是BeanValidation2.0，这里的Bean就是我们一直在说的实例化后的POJO类
```
<dependency>
  <groupId>jakarta.validation</groupId>
  <artifactId>jakarta.validation-api</artifactId>
  <version>2.0.1</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

###Validation注解
这些注解可以直接设置在Bean的属性上
* @NotNull：不允许为null对象
* @AssertTrue：是否为true
* @Size：约定字符串的长度
* @Min：字符串的最大长度
* @Max：字符串的最小长度
* @Email：是否是邮箱格式
* @NotEmpty：不允许为null或者为空，可以用于判断字符串，集合
* @NotBlank：不允许为null和空格

```
package com.bookstore.model;

import javax.validation.constraints.*;

public class User {

    @NotEmpty(message = "名称不能为 null")
    private String name;

    @Min(value = 18, message = "你的年龄必须大于等于18岁")
    @Max(value = 150, message = "你的年龄必须小于等于150岁")
    private int age;

    @NotEmpty(message = "邮箱必须输入")
    @Email(message = "邮箱不正确")
    private String email;
 
    // standard setters and getters 
}
```

#数据校验步骤
1. 创建一个表单页
2. 执行校验




###创建表单
创建一个user/addUser.html模板文件，用于管理员添加用户
```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="X-UA-Compatible" content="ie=edge" />
  <title>添加用户</title>
</head>

<body>
  <h2>添加用户</h2>
  <form action="/user/save" method="POST">
    <div>
      <label>用户名称:</label>
      <input type="text" name="name">
    </div>
    <div>
      <label>年龄:</label>
      <input type="text" name="age">
    </div>
    <div>
      <label>邮箱:</label>
      <input type="text" name="email">
    </div>
    <div>
      <button type="submit">保存</button>
    </div>
  </form>
</body>

</html>
```
mapping：在control类里设置mapping
```
import javax.validation.Valid;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;
import org.springframework.validation.BindingResult;
import com.bookstore.model.*;

@Controller
public class UserControl {

  @GetMapping("/user/add.html")
  public String addUser() {
    return "user/addUser";
  }

}
```

###执行校验
```
package com.bookstore.control;

import com.bookstore.model.User;
import org.springframework.stereotype.Controller;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

import javax.validation.Valid;

@Controller
public class UserControl {

    @GetMapping("/user/add.html")
    public String addUser() {
        return "user/addUser";
    }

    @PostMapping("/user/save")
    public String saveUser(@Valid User user, BindingResult errors) {
        if (errors.hasErrors()) {
            // 如果校验不通过，返回用户编辑页面
            return "user/addUser";
        }
        // 校验通过，返回成功页面
        return "user/addUserSuccess";
    }

}
```


##返回出错原因
1. Control改造
2. user/add.html改造

Control：
```
@GetMapping("/user/add.html")
public String addUser(Model model) {
    User user = new User();
    model.addAttribute("user",user);
    return "user/addUser";
}
```

the:object：用于替换对象，使用了后就不用每次都编写user.xxx，可以直接操作xxx
```
<form action="/user/save" th:object="${user}" method="POST">
  ...
</form>
```

th:classappend：支持动态的管理样式
```
<div th:classappend="${#fields.hasErrors('name')} ? 'error' : ''">
</div>
```
${#fields.hasErrors('key')}这个语法是专门为验证场景提供的，key是对象属性的名称

th:errors：显示出错误信息，使用`th:errors="*{age}"属性
```
<p th:if="${#fields.hasErrors('age')}" th:errors="*{age}"></p>
```

th:field：显示上次输入的内容
```
<div th:classappend="${#fields.hasErrors('age')} ? 'error' : ''">
  <label>年龄:</label>
  <input type="text" th:field="*{age}" />
  <p th:if="${#fields.hasErrors('age')}" th:errors="*{age}"></p>
</div>
```

##Thymeleaf布局（Layout）
<img src="https://style.youkeda.com/img/ham/course/j4/book/layout.svg" alt= "image"/>
使用th:include + th:replace方案来完成布局的开发

```html
layout.html:
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>布局</title>
    <style>
        .header {background-color: #f5f5f5;padding: 20px;}
        .header a {padding: 0 20px;}
        .container {padding: 20px;margin:20px auto;}
        .footer {height: 40px;background-color: #f5f5f5;border-top: 1px solid #ddd;padding: 20px;}
    </style>
</head>
<body>
<header class="header">
    <div>
        <a href="/book/list.html">图书管理</a>
        <a href="/user/list.html">用户管理</a>
    </div>
</header>
<div class="container" th:include="::content">页面正文内容</div>
<footer class="footer">
    <div>
        <p style="float: left">&copy; youkeda.com 2017</p>
        <p style="float: right">
            Powered by 优课达
        </p>
    </div>
</footer>

</body>
```
th:include="::content"：
::content指的是选择器，这个选择器指的是加载单签页面的th:fragment的值，当页面渲染的时候，布局会合并content这个fragment一起渲染

```html
user/list.html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org"
      th:replace="layout">
<div th:fragment="content">
    <h2>用户列表</h2>
    <div>
        <a href="/user/add.html">添加用户</a>
    </div>
    <table>
        <thead>
        <tr>
            <th>
                用户名称
            </th>
            <th>
                用户年龄
            </th>
            <th>
                用户邮箱
            </th>
        </tr>
        </thead>
        <tbody>
        <tr th:each="user : ${users}">
            <td th:text="${user.name}"></td>
            <td th:text="${user.age}"></td>
            <td th:text="${user.email}"></td>
        </tr>
        </tbody>
    </table>
</div>

</html>
```
th:replace="layout":
指定了布局的名称，一旦声明后，页面就会被替换成layout的内容，这个“layout”指的是templates/layout.html

th:fragment="content"
```
<div th:fragment="content">
</div>
```
fragment是片段的意思，当页面渲染的时候，可以通过选择器制定使用这个片段，在上面的layout.html文件的th:include="::content"指定的就是这个值


```
<div th:fragment="content">

  <script>
     function delBook(id){

        var r = confirm("确定删除本书么？");
        if(!r){
            return;
        }

        ...使用 fetch 进行 api 请求

        //执行成功后刷新页面
        window.location.href = "/book/list.html";

     }
  </script>
</div>




<table>
  <thead>
    <tr>
      ...
      <th>操作</th>
    </tr>
  </thead>
  <tbody>
    <tr th:each="book : ${books}">
      ...
      <td>
        <a href="javascript:;" th:onclick="delBook([[${book.id}]])">删除</a>
      </td>
    </tr>
  </tbody>
</table>
```

###删除操作
步骤一：
```
Iterator<Book> itr = books.iterator();
while (itr.hasNext()) {
    Book book = itr.next();
    if (book.getId() == bookId) {
        itr.remove();
    }
}
```

步骤二：添加js函数
```
<div th:fragment="content">

  <script>
     function delBook(id){

        var r = confirm("确定删除本书么？");
        if(!r){
            return;
        }

        ...使用 fetch 进行 api 请求

        //执行成功后刷新页面
        window.location.href = "/book/list.html";

     }
  </script>
</div>
```

操作删除
```
<table>
  <thead>
    <tr>
      ...
      <th>操作</th>
    </tr>
  </thead>
  <tbody>
    <tr th:each="book : ${books}">
      ...
      <td>
        <a href="javascript:;" th:onclick="delBook([[${book.id}]])">删除</a>
      </td>
    </tr>
  </tbody>
</table>
```

完整代码：
```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org"
      th:replace="layout">
<div th:fragment="content">
    <h2>图书列表</h2>
    <div>
        <a href="/book/add.html">添加图书</a>
    </div>
    <table>
        <thead>
        <tr>
            <th>
                图书名称
            </th>
            <th>
                图书作者
            </th>
            <th>
                图书描述
            </th>
            <th>
                图书编号
            </th>
            <th>
                图书价格
            </th>
            <th>图书封面</th>
            <th>操作</th>
        </tr>
        </thead>
        <tbody>
        <tr th:each="book : ${books}">
            <td th:text="${book.name}"></td>
            <td th:text="${book.author}"></td>
            <td th:text="${book.desc}"></td>
            <td th:text="${book.isbn}"></td>
            <td th:text="${book.price}"></td>
            <td th:text="${book.pictureUrl}"></td>
            <td><a href="javascript:;" th:onclick="delBook([[${book.id}]])">删除</a></td>
        </tr>
        </tbody>

    </table>


    <script>
     function delBook(id){

        var r = confirm("确定删除本书么？");
        if(!r){
            return;
        }

        fetch("/book/del?id="+id).then(res=>res.json()).then(res=>{
            if(res){
                alert("删除成功");
                // 刷新页面
                window.location.href = "/book/list.html";
            }
        })
     }
    </script>
</div>

</html>
```
一般，我们都会设置表格的最后一列为操作列
这里还有一个技巧，由于超链接有默认打开链接行为，所以对于想要执行onclick的时候，我们一般设置href="javascript:;"





















