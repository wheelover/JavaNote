#Why choose MySQl
##1.1什么是数据库
<ul>
<li>将大量数据保存起来，通过计算机加工而成的可以进行高效访问的数据集合称为数据库 (DataBase，简称 DB)</li>
<li>用来管理数据库的计算机系统称为数据库管理系统 (DataBase Management System，简称DBMS)。</li>
<li>从事管理和维护数据库管理系统(DBMS)的相关工作人员称为数据库管理员（Database Administrator 简称DBA）</li>
</ul>

数据库模型主要是两种，即关系型数据库和非关系型数据库

关系型数据库模型是把复杂的数据结构归结为简单的二元关系，在数据库中，对数据的操作几乎全部建立在一个或多个关系表格上，通过对这些关联表的表格分类，合并，连接或选取等来实现数据的管理

![](https://qgt-style.oss-cn-hangzhou.aliyuncs.com/newcoursep4/d1/d1-1/%E7%94%BB%E6%9D%BF.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

##1.2数据库分类
SQL（Structured Query Language）结构化查询数据

NoSQL（非关系型数据库）
NoSQL不保证关系数据的ACID特性

Atomicity（原子性）：一个事务（transaction）中的所有操作，或者全部完成，或者全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。即，事务不可分割、不可约简。

Consistency（一致性）：在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设约束、触发器、级联回滚等。

Isolation（隔离性）：数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括未提交读（Read uncommitted）、提交读（read committed）、可重复读（repeatable read）和串行化（Serializable）。

Durability（持久性）：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。

NoSQL：我们将学习Redis和MongoDB


##1.3为什么选择MySQL
排名前三的关系型数据库
Oracle-甲骨文
SQL Server-微软
MySQL-甲骨文

<table>
<thead>
<tr>
<th></th>
<th>Oracle</th>
<th>SQL Server</th>
<th>MySQL</th>
</tr>
</thead>
<tbody><tr>
<td>价格</td>
<td>昂贵</td>
<td>贵</td>
<td>免费</td>
</tr>
<tr>
<td>性能</td>
<td>功能齐全，稳定性极好</td>
<td>Win 系统下功能齐全，稳定性好，界面友好</td>
<td>功能有限，但足够用，并且可以魔改</td>
</tr>
<tr>
<td>主要用户</td>
<td>世界五百强，金融行业</td>
<td>Win 系统下软件商</td>
<td>互联网公司</td>
</tr>
</tbody></table>




##1.4安装并启动MySQL
安装MySQL软件，分别执行下列命令
```
sudo docker run -itd -p 3306:3306  \
    -e MYSQL_ALLOW_EMPTY_PASSWORD="root" \
    --name mysql \
    mysql \
    --character-set-server=utf8 \
    --collation-server=utf8_general_ci \
    --default-authentication-plugin=mysql_native_password
```

3306是MySQL服务的端口号

进入服务
```
sudo docker exec -it mysql bash
```

登录MySQL
```
mysql -uroot
```

创建数据库
```
CREATE DATABASE youkedadb;(polor)
```

登出MySQL
```
\quit
```

退出服务
```
exit
```

启动不正常
```
sudo docker ps -a
```

如果看到有名字为mysql的记录，说明处于暂停状态，执行
```
sudo docker start mysql
```


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

























