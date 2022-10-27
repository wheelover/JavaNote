#Spring Data入门
使用Docker安装MongoDB软件
##安装MongoDB
###安装
```
sudo docker run -itd --name mongo -p 27017:27017 mongo
```
###验证
```
sudo docker exec -it mongo mongo admin
```

###创建数据库
```
use practice
```

###退出登录
```
exit
```

##重启MongoDB
###查看MongoDB的启动情况
```
sudo docker ps
```
如果看到一条名字为mongo的记录，其端口是27017，就表示正常启动了
<img src="https://style.youkeda.com/img/ham/course/d2/d3-1-3-2.1.png?x-oss-process=image/resize,w_1024/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image"/>

###启动不正常
先执行
```
sudo docker ps -a
```
如果看到有名字为mongo的记录，说明当前处于暂停状态，执行
```
sudo docker start mongo
```
仍然没有记录，可能由于长时间没有使用平台，被自动移除了，就要重新安装了


##Spring Data MongoDB配置
依赖添加
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```
也可以在`https://start.spring.io/`创建工程的时候添加选项
<img src="https://style.youkeda.com/img/ham/course/j4/j4-8-2-1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image"/>

###配置
修改`src/main/resources/application.properties`文件，增加配置项
```
# 购买的云服务器的公网 IP
spring.data.mongodb.host=192.168.0.1
# MongoDB 服务的端口号
spring.data.mongodb.port=27017
# 创建的数据库及用户名和密码
spring.data.mongodb.database=practice
```

###云服务器安全设置
<img src="https://style.youkeda.com/img/ham/course/j4/j4-8-2-2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" alt="image"/>

##Spring Data CRUD
对数据库的操作一定要放在@Service类中，而不是放在@Controller类中，且@Controller类可以调用@Service类的方法，反之不行，这是SpringMVC的经典框架设计

###1.新增数据
向数据库插入数据(实例对象)
```Java
import org.springframework.data.mongodb.core.MongoTemplate;

  @Autowired
  private MongoTemplate mongoTemplate;

  public void test() {
    Song song = new Song();
    song.setSubjectId("s001");
    song.setLyrics("...");
    song.setName("成都");

    mongoTemplate.insert(song);
  }
```
让系统自动注入MongoTemplate的示例，调用mongoTemplate.insert()方法把对象存入数据中

###2.查询数据
```Java
mongoTemplate.findById(songId, Song.class)
```
findById()方法第一参数就是主键id，第二个参数是具体的类，写法是`类名.class`

###3.修改数据(根据主键id查询数据)
分两步：
1. 修改那条数据
2. 那个字段修改成什么值
```Java
// 修改 id=1 的数据
Query query = new Query(Criteria.where("id").is("1"));

// 把歌名修改为 “new name”
Update updateData = new Update();
updateData.set("name", "new name");

// 执行修改，修改返回结果的是一个对象
UpdateResult result = mongoTemplate.updateFirst(query, updateData, Song.class);
// 修改的记录数大于 0 ，表示修改成功
System.out.println("修改的数据记录数量：" + result.getModifiedCount());
```

###4.删除数据
```Java
Song song = new Song();
song.setId(songId);

// 执行删除
DeleteResult result = mongoTemplate.remove(song);
// 删除的记录数大于 0 ，表示删除成功
System.out.println("删除的数据记录数量：" + result.getDeletedCount());
```

##Spring Data Query
条件查询的核心方法
```Java
List<Song> songs = mongoTemplate.find(query, Song.class);
```
因为可能查询到多条数据，所以结果是对象的集合
查询方法比较简单，但查询操作的复杂性在于条件，需要用构建好的Criteria条件对象的示例，来构建Query实例
构建Criteria条件对象，一般有两种情况：
* 单一条件，用：
```
Criteria criteria1 = Criteria.where("条件字段名").is("条件值")
```
* 组合条件，根据或(or),且(and)的关系进行组合，多个子条件对象组合成一个总条件
或关系
```
Criteria criteria = new Criteria();
criteria.orOperator(criteria1, criteria2);
```
且关系
```
Criteria criteria = new Criteria();
criteria.andOperator(criteria1, criteria2);
```
orOperator()和andOperator()的参数，都可以输入多个子条件，也可以输入子条件数组

示例(根据歌曲的专辑查询)，并限定最多返回10条数据
```Java
import org.springframework.data.mongodb.core.query.Query;
import org.springframework.data.mongodb.core.query.Criteria;

public List<Song> list(Song songParam) {
    // 总条件
    Criteria criteria = new Criteria();
    // 可能有多个子条件
    List<Criteria> subCris = new ArrayList();
    if (StringUtils.hasText(songParam.getName())) {
      subCris.add(Criteria.where("name").is(songParam.getName()));
    }

    if (StringUtils.hasText(songParam.getLyrics())) {
      subCris.add(Criteria.where("lyrics").is(songParam.getLyrics()));
    }

    if (StringUtils.hasText(songParam.getSubjectId())) {
      		subCris.add(Criteria.where("subjectId").is(songParam.getSubjectId()));
    }

    // 必须至少有一个查询条件
    if (subCris.isEmpty()) {
      LOG.error("input song query param is not correct.");
      return null;
    }

    // 三个子条件以 and 关键词连接成总条件对象，相当于 name='' and lyrics='' and subjectId=''
    criteria.andOperator(subCris.toArray(new Criteria[]{}));

    // 条件对象构建查询对象
    Query query = new Query(criteria);
    // 仅演示：由于很多同学都在运行演示程序，所以需要限定输出，以免查询数据量太大
    query.limit(10);
    List<Song> songs = mongoTemplate.find(query, Song.class);

    return songs;
}
```

8.5作业
```Java
Control:
 @RequestMapping("/songlist")
  public Map list(String subjectId, String name, String lyrics) {
    Map returnData = new HashMap();
    Song queryParam = new Song();
    queryParam.setSubjectId(subjectId);
    queryParam.setLyrics(lyrics);
    queryParam.setName(name);
    List<Song> songResult = songService.list(queryParam);

    returnData.put("songResult", songResult);
    return returnData;
  }
  
  
  Impl:
    @Override
  public List<Song> list(Song songParam) {
    // 作为服务，要对入参进行判断，不能假设被调用时，入参一定正确
    if (songParam == null) {
      LOG.error("input song data is not correct.");
      return null;
    }

    // 总条件
    Criteria criteria = new Criteria();
    // 可能有多个子条件
    List<Criteria> subCris = new ArrayList();
    if (StringUtils.hasText(songParam.getName())) {
      subCris.add(Criteria.where("name").is(songParam.getName()));
    }

    if (StringUtils.hasText(songParam.getLyrics())) {
      subCris.add(Criteria.where("lyrics").is(songParam.getLyrics()));
    }

    if (StringUtils.hasText(songParam.getSubjectId())) {
      subCris.add(Criteria.where("subjectId").is(songParam.getSubjectId()));
    }

    // 必须至少有一个查询条件
    if (subCris.isEmpty()) {
      LOG.error("input song query param is not correct.");
      return null;
    }

    // 三个子条件以 and 关键词连接成总条件对象，相当于 name='' and lyrics='' and subjectId=''
    criteria.andOperator(subCris.toArray(new Criteria[]{}));

    // 条件对象构建查询对象
    Query query = new Query(criteria);
    // 仅演示：由于很多同学都在运行演示程序，所以需要限定输出，以免查询数据量太大
    query.limit(10);
    List<Song> songs = mongoTemplate.find(query, Song.class);

    return songs;
  }

```


##Spring Data分页
查询支持分页，只需要调用PageRequest.of()方法构建一个分页对象，然后注入到查询对象即可
PageRequest.of()方法第一个参数是页码，注意从0开始技术，第一页的值是0，第二参数是每页的数量

```Java
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;

Pageable pageable = PageRequest.of(0 , 20);
query.with(pageable);
```
对于分页来说除了要查询结果意外，还要查询总数，才能进一步计算出总共多少页，实现完整的分页功能
* 调用count(query, XXX.class)方法查询总数，第一个参数是查询条件，第二参数是表示查询什么样的对象
* 根据结果，分页条件，总数三个数据，构建分页器的对象
```Java
import org.springframework.data.domain.Page;
import org.springframework.data.repository.support.PageableExecutionUtils;

// 总数
long count = mongoTemplate.count(query, Song.class);
// 构建分页器
Page<Song> pageResult = PageableExecutionUtils.getPage(songs, pageable, new LongSupplier() {
  @Override
  public long getAsLong() {
    return count;
  }
});
```
PageableExecutionUtils.getPage()方法第一个参数是查询结果，第二个参数是分页条件对象，第三个是实现一个LongSupplier接口的匿名类，在匿名类的getAsLong()方法中返回结果总数。方法返回值是一个Page分页对象

control中调用SongService.list()方法的分页结果对象后，可以调用Page的各个方法取得数据集和前端分页器的各种数据值
```
Page<Song> songResult = songService.list(queryParam);
List<Song> songs = songResult.getContent();
```









































































