#CRUD(Create, Read, Update, Delete)
##2.1表格结构
在云服务器上可以创建很多DB(database)，表就放在配置的数据库下

表名：在MySQL中我们一般使用英文小写字母来约定表名，比如用户表我们命名为user，多个单词用`_`分割，不可重复

字段：每一列都是一个字段，第一行是字段名，下面的就是字段的值，字段必须是唯一的，不能同名

主键：每一张数据库表都可以有一个主键，最大作用是用来标识数据
<ul>
<li>主键是一个特殊字段</li>
<li>表格可以没有主键，但是最多只能拥有<font color="red">一个</font>主键</li>
<li>主键的值不能为<code>NULL</code>，必须有对应的值</li>
<li>主键的值必须是<font color="red">绝对唯一</font>的，即不能出现两个相同的主键值，比如名字就不能作为主键，因为会出现重名的情况。</li>
<li>我们一般使用主键和其他表进行关联</li>
</ul>

SQL常用数据类型
<table>
<thead>
<tr>
<th>类型</th>
<th>含义</th>
</tr>
</thead>
<tbody><tr>
<td>VARCHAR</td>
<td>可变的长字符串，可以类比于Java中的String类型</td>
</tr>
<tr>
<td>INT</td>
<td>整型，和Java中的int类型一致</td>
</tr>
<tr>
<td>DOUBLE</td>
<td>浮点型，和Java中的double类型一致,一般不加长度限制</td>
</tr>
<tr>
<td>DATETIME</td>
<td>时间类型，长度为0，格式为YYYY-MM-DD HH:MM:SS，值为2019-12-31 23:59:59</td>
</tr>
<tr>
<td>BIGINT</td>
<td>长整形，和Java中的long类型一致</td>
</tr>
</tbody></table>


数据：与主键关联，相当于HashMap中的key与value


在SQL中，拥有对应的专业术语
<table>
<thead>
<tr>
<th>英文</th>
<th>中文</th>
<th>SQL</th>
<th>HTTP</th>
</tr>
</thead>
<tbody><tr>
<td>CREATE</td>
<td>创建</td>
<td>INSERT（插入）</td>
<td>POST</td>
</tr>
<tr>
<td>READ</td>
<td>读取</td>
<td>SELECT（查询）</td>
<td>GET</td>
</tr>
<tr>
<td>UPDATE</td>
<td>更新</td>
<td>UPDATE</td>
<td>POST</td>
</tr>
<tr>
<td>DELETE</td>
<td>删除</td>
<td>DELETE</td>
<td>DELETE</td>
</tr>
</tbody></table>


##2.2创建表格
创建表格是我们需要提供以下属性：
* 表名
* 字段名
* 字段的数据类型

创建表格的代码
```sql
CREATE TABLE `user`(
`id` INT(10) NOT NULL,
`mobile` VARCHAR(11) NOT NULL,
`nickname` VARCHAR(40) NOT NULL,
`gmt_created` datetime ,
`gmt_modified` datetime NOT NULL,
PRIMARY KEY ( `id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
他们的语法结构都是一样的即：**字段名+数据类型+长度+是否为NULL**

<ul>
<li><code>id</code>是字段名，我们用``这个符号把它包含起来。</li>
<li>INT 是数据类型，表示 id 这个字段是 INT 值</li>
<li>(10)表示 id 最长为 10 位</li>
<li>datetime 类型没有长度，所以不用定义长度</li>
<li>NOT NULL 表示这个字段不能为空，也就是必须要输入值，否则就会报错，如果这个值可以为不输入值，那么不加 NOT NULL</li>
</ul>

INT的最大值为2147483647，长度为10位

###约定主键
```
PRIMARY KEY ( `id` )
```

有时候我们会把INT类型的主键语句改为
```
`id` INT UNSIGNED AUTO_INCREMENT
```
这句话的意思是，id会从1开始自增，第二个为2，第三个为3


###设置存储引擎和编码方式
```
ENGINE=InnoDB DEFAULT CHARSET=utf8
```
InnoDB是MySQL的默认存储引擎，utf-8是一种编码方式

###关键词和保留字
在编译器里，变色了的单词就是关键词和保留字
![](https://qgt-style.oss-cn-hangzhou.aliyuncs.com/newcoursep4/d1/d1-3/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A120200526110137.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
遇到关键字和保留字的时候，用``` ` ```这个符号去过滤掉它

###删除表格
删除表格的语法
```
drop table table_name;
```
也可以这样写
```
DROP TABLE IF EXISTS table_name;
```

table_name是指要删除的表格名


###查看表结构
```
mysql -h 192.168.0.1 -uroot -Dyoukedadb -e 'desc winner;'
```
如果SQL语句太长用
```
mysql -h 192.168.0.1 -uroot -Dyoukedadb < index.sql;
```
简单SQL
```
mysql -h 192.168.0.1 -uroot -Dyoukedadb -e 'select * from timi_adc;'
```


<blockquote>
<ol>
<li>192.168.0.1 是优课达平台 IP，固定的不用改</li>
<li>-uroot 表示使用 root 用户</li>
<li>-Dyoukedadb 表示操作的是第一章创建好的 youkedadb 库</li>
<li>-e 表示执行后面单引号内的 SQL 语句</li>
</ol>
</blockquote>

<p><code>desc winner;</code> 是查看表格结构的 SQL 语句。如果查看其它表结构，只要把 <code>winner</code> 替换成其它表名即可。</p>

<blockquote>
<p>重点是记住 <code>desc 表名;</code> 语法</p>
</blockquote>


##2.3插入语句(INSERT)
###语法
```sql
INSERT INTO table_name(field1,field2,...fieldN)
VALUES
(value1,value2,...valueN);
```
我们向制定表插入若干个字段，和他们对应的值
```sql
INSERT INTO
  `user` (`id`, `mobile`, `nickname`, `gmt_created`)
VALUES
  (1, '13426069530', '叶冰', now());
```
* user是表名
* id，mobile等是字段名
* mobile的值是VARCHAR类型，所以要用' '包含
* gmt_create是datetime类型，我们一般使用now()这个函数来获取服务器的时间，并插入

###插入语句的简化
1. 如果主键设置为自增，则可以不插入主键和对应的数据
2. 如果插入的是所有的字段，可以省略字段名，直接插入值，但是类型必须全部一致
```sql
INSERT INTO table_name
VALUES
(value1,value2,...valueN);
```

###批量插入数据
```sql
INSERT INTO table_name
VALUES
(value1,value2,...valueN),
(value1,value2,...valueN);
```

##2.4查询(SELECT)
###语法
```
SELECT field1,field2,.... FROM table_name;
```

从指定表中查询制定列的信息
```
SELECT
  id,
  hero_name
FROM
  timi_adc;
```

查询所有的字段
```
SELECT * FROM timi_adc;
```

###WHERE句子
在MySQL使用WHERE语句来限定条件，作用类似程序语言中的if语句

###语法
```
SELECT * FROM table_name WHERE condition;
```
condition指的是条件，它和if语句一样，可以做简单的逻辑判断，我们查询到的数据是true

###应用
查询胜率超过50%的射手
```
SELECT
  *
FROM
  timi_adc
where
  win_rate > 0.5;
```

##2.5Limit子句
我们想查询符合某个条件的前十个数据，就需要使用LIMIT子句来加强查询功能
```
SELECT * FROM table_name LIMIT parameter;
```

###查询第x-y行
```
SELECT
  *
FROM
  timi_adc
LIMIT
  5, 6;
```
这句的意思是查询timi_adc表的第6-11行，第一行参数5表示从第6行开始查，第二个参数6，表示一共查询6条记录

数据库的表格从第0行开始数

###查询0-x行
```
SELECT
  *
FROM
  timi_adc
LIMIT
  5;
```
这句的意思是查询user表的0-5行

###查询第x行
```
SELECT
  *
FROM
  timi_adc
LIMIT
  4, 1;
```
这句SQL的意思是查询timi_adc表第5行的数据

**LIMIT和WHERE子句联合使用**
比如要查下timi_adc表中登场率大于10%的前五条数据
```
SELECT
  *
FROM
  timi_adc
WHERE
  appearance_rate > 0.1
LIMIT
  5;
```

##2.6排序(ORDER BY子句)

对查询结果进行排序

###语法
```
SELECT * FROM table_name ORDER BY field_name;
```

```
SELECT
  *
FROM
  timi_adc
ORDER BY
  win_rate;
```
排序默认按照升序排列

###DESC关键词
ORDER BY语句的排序默认是正序排列，关键词为ASC(一般不写)，我们可以通过加上关键词DESC，使得排序变成逆序
```
SELECT
  *
FROM
  timi_adc
ORDER BY
  win_rate DESC;
```

###与其他句子连用
```
SELECT
  *
FROM
  timi_adc
ORDER BY
  win_rate DESC
LIMIT
  3;
```

##2.7更新
###语法
```
UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值
```
UPDATE语句必须加入WHERE限定条件，否则的话UPDATE语句就会对整列起作用

将timi_adc表格中的艾琳这个英雄的ban率改为1%
```
UPDATE
  timi_adc
SET
  ban_rate = 0.01
WHERE
  hero_name = '艾琳';
```

###删除语句(DELETE)
```
DELETE FROM table_name [WHERE Clause]
```
必须增加WHERE语句，否则会把整张表删掉

DELET的应用场景
删除user表中id为4的行
```
delete from user where id=4;
```

删除user表中所有id小于20的数据
```
delete from `user` where id<20;
```

删除user表中的所有数据
```
delete from user;
```
DELETE语句只删除表中的数据


##2.8备份与恢复
输入备份命令
```
mysqldump -h 192.168.0.1 -uroot youkedadb > youkedadb.sql
```
即可吧整个youkedadb数据库中的数据导出

恢复数据
打开**具备备份文件的作业**。执行一下命令
```
mysql -h 192.168.0.1 -uroot -Dyoukedadb<youkedadb.sql
```






