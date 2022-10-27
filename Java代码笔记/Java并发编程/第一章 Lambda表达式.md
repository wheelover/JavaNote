#第一章Lambda表达式
##1.1简介
<p>Java 8 是 Java 语言发展史上的一个重要里程碑，更新了许多新特性，包括 <b>Lambda</b>、<b>流</b>和<b>函数式编程</b>等等。这些特性可以使代码更简洁、易阅读、易维护。</p>

##1.2无类型参数
Lambda表达的基本结构：
```
f -> { }
```

![](https://style.youkeda.com/img/ham/course/j5/j5-1-2-1.svg)

Lambda表达式在功能上等同于一个匿名方法：
```
public void unknown(f) {
  System.out.println(f.getName());
}
```

对比来说就是方法名和public void修饰符都省略了

###类型识别
f变量的类型由系统根据上下文自动识别

```java
List<Fruit> fruits = Arrays.asList(......);

fruits.forEach(f -> {
  System.out.println(f.getName());
});
```

forEach()方法表示循环遍历fruits集合

Lambda表达式要配合上下文，跟其他方法配合使用，而不是一个独立语句
```
public static void main(String[] args) {
  args[0] -> {
    System.out.println(args[0]);
  }
}
```

###实战案例
演示Collections中sort()方法的排序功能
```
List<Student> students = new ArrayList<Student>();
students.add(new Student(111, "bbbb", "london"));
students.add(new Student(131, "aaaa", "nyc"));
students.add(new Student(121, "cccc", "jaipur"));

// 实现升序排序
Collections.sort(students, (student1, student2) -> {
  // 第一个参数的学号 vs 第二个参数的学号
  return student1.getRollNo() - student2.getRollNo();
});

students.forEach(s -> System.out.println(s));
```

Collections.sort()方法第二个参数是实现匿名类new Comparator(){}

###多参数
有**多个参数**时候，必须使用小括号包裹
```
(student1, student2) -> {}
```

###无参数
**没有参数**的时候，比逊使用小括号()
```
() -> {}
```

###单条执行语句
箭头后的执行语句**只有一条**时，可以不加大括号包裹
```
s -> System.out.println(s);
```

##1.3有类型参数
```
fruits.forEach((Fruit f) -> {
  System.out.println(f.getName());
});
```
即使只有一个参数，也必须使用小括号()

##1.4引用外部变量
Lambda表达式{}内的执行语句，除了能引用参数变量意外，还可以引用外部的变量
```
List<Fruit> fruits = Arrays.asList(......);
String message = "水果名称：";

fruits.forEach(f -> {
  System.out.println(message + f.getName());
});

```

###规范一：
Lambda表达式有规范：引用的**局部变量**不允许被修改，即使写在表达式后面也不行
下面代码修改了局部变量也是错误的
```
String message;

fruits.forEach(f -> {
  message = "水果名称：";
});
```
这样也是错误的
```
List<Fruit> fruits = Arrays.asList(......);
String message = "水果名称：";

fruits.forEach(f -> {
  System.out.println(message + f.getName());
});

message = "";
```
Lambda表达式引用的局部变量即使不声明为final，也具备final的特性：变量值初始化后不允许被修改

在表达式外实际值赋值一次的写法是可以的
```
String message;
message = "水果名称：";
```

###规范二：
参数不能与**局部变量**同名
下面写法是错误的
```
String f = "水果名称：";
fruits.forEach(f -> {});
```

##1.5双冒号(::)操作符
只有一条执行语句的Lambda表达式
```
List<String> names = Arrays.asList("zhangSan", "LiSi", "WangWu");

names.forEach(n -> {
  System.out.println(n);
});
```

可以进一步简化为
```
names.forEach(System.out::println);
```
使用`::`时，系统每次遍历取得元素n会自动作为参数传递给`System.out.println()`方法打印输出

###语法含义
![](https://style.youkeda.com/img/ham/course/j5/j5-1-5-1.svg)

###不同用法

**用法一：**静态方法调用
使用`LambdaTest::print`代替`f -> LambdaTest.print(f)`

**用法二：**调用非静态方法
`print()`方法不载表示为`static`，于是需要实例对象来调用
```
fruits.forEach(new LambdaTest()::print);
```

只是简写了
```
LambdaTest lt = new LambdaTest();
fruits.forEach(lt::print);
```

<p>实际上，本节开头的案例中，<code>System.out::println</code> 语句就是调用非静态方法 <code>println()</code>，因为 <code>System.out</code> 指代的是一个实例对象。只是这个实例对象比较复杂，不是本课程重点，这里不用纠结。</p>

**用法三：多参数**
```
Collections.sort(students, (student1, student2) -> {
  // 第一个参数的学号 vs 第二个参数的学号
  return student1.getRollNo() - student2.getRollNo();
});
```
这里碰到了多参数的情况，如果把比较的过程定义成一个方法
```
private static int compute(Student s1, Student s2) {
  ... ...
  ... ...
}
```

那么，排序过程就可以简写为
```
Collections.sort(students, SortTest::compute);
```

注意：系统会自动获取上下文的参数，并按上下文定义的顺序传递给指定的方法，所谓顺序就是Lambda表达式`()`中的顺序。与变量名无关，s1 s2命名只是便于大家理解

**用法四：父类方法**
super关键字的作用是在子类中引用父类的属性或方法。`::`语法也可以用`super`关键字调用父类的非静态方法

```
import java.util.Arrays;
import java.util.List;

public class LambdaTest extends LambdaExample {
  public static void main(String[] args) {
    List<Fruit> fruits = Arrays.asList(
            new Fruit("香蕉"),
            new Fruit("苹果"),
            new Fruit("梨子"),
            new Fruit("西瓜"),
            new Fruit("荔枝")
    );

    LambdaTest at = new LambdaTest();
    at.print(fruits);
  }

  public void print(List<Fruit> fruits){
    fruits.forEach(super::print);
    }
}

class LambdaExample {
    public void print(Fruit f){
        System.out.println(f.getName());
    }
}
```

























































