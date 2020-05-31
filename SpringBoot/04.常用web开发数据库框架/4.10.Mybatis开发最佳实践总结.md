# 一、 面对场景的最佳实践

mybatis代码实现方式

1. 使用mybatis generator做代码自动生成
2. 使用XML方式实现
3. 使用注解方式实现

以上三种实现方式，有自己适合的应用场景，按照4.9章节中集成方式，三种方法全部可以支持。下面是结合笔者多年的mybatis使用经验，总结出在不同的场景下，使用不同的实现方式

## 场景一：单表的增删改查

mybatis generator生成的代码能够完成90%的**单表操作**，而且不用自己去书写SQL。使用非常方便！
学习可以参考:[如何使用Mybatis自动生成的代码](https://www.kancloud.cn/hanxt/springboot2/1177661)

> 这种用法面对开发人员非常友好，有的人说经常用这个会忘记怎么写SQL。我可以斩钉截铁的回答：不会的，因为你脑袋里面没有SQL，是用不明白这种方式的。但是这种用法虽然简单易用，也会产生一个问题，就是通常写一个关联查询就可以得到的结果，开发人员会倾向于用多次使用单表查询（因为写起来简单，可以犯懒）。说实话性能倒不会一定下降，但代码会很冗余。项目组如果想避免这种情况发生，要特意强调做好规范。

**自动生成的代码 最大程度帮你完成单表操作。涉及到关联查询、继承，Mybatis文件和SQL还是要你自己写，但是不要在生成的代码基础上面改！切记！** 如果使用自动代码生成感觉不适合自己或自己的项目，使用类似于Mybatis-Plus这种第三方增强库，也是很方便的。

## 场景二: 多查询条件的查询(或多表关联查询)

在web开发中，有一个典型的应用场景是：一个web table页面有多个查询条件，选择填写不同的查询条件得到不同的查询结果，多个查询条件只填写几个条件进行查询，其他的条件不填写。
面对这种场景，就需要ORM框架对 **动态SQL(根据传入参数不同，SQL会发生变化)** 有很好的支持，包括书写的方便度等。从这个角度上讲，mybatis的xml的是实现方式独占鳌头。

```xml
<select id="findStudent" parameterType="int" resultType="com.zimug.bootlaunch.testdb1.model.Student">
        SELECT STUD_ID AS studId,name,email,dob
        FROM STUDENT
        <trim prefix="WHERE" prefixOverrides="AND|OR" suffixOverrides="AND|OR">
            <if test="stuId != null" >
            AND STUD_ID = #{stuId}
            </if>
            <if test="name != null and name != '' " >
                AND name = #{name}
            </if>
            <if test="email != null and email != '' " >
                AND email= #{email}
            </if>
        </trim>
    </select>
    List<Student> findStudent(@Param("stuId") Integer stuId,
                              @Param("name") String name,
                              @Param("email") String email);
```

另外如果你做一个多表的关联查询，不需要使用动态SQL的情况下，使用XML方式也是一个不错的选择。比起在注解方式里面字符串拼SQL要更好。

## 场景三: 除上面两种场景外的其他场景

其实除去上面两种场景，剩下的情况已经不多了，但是还是可以举几个例子：
比如：针对单表只有插入操作，你又不想因此生成一套完整的针对单表的操作代码。
比如：只是临时起意，写一个较为简单的SQL。

```java
public interface AnonStudentMapper {
    @Select("SELECT STUD_ID AS studId, NAME, EMAIL, DOB " +
            "FROM STUDENT " +
            "WHERE STUD_ID=#{studId}")
    List<Student> findStudentById(Integer studId);
}
```

可以看到这种方式，最好是SQL在两三行以内，并且没有嵌套SQL，否则会陷入维护的灾难！

# 二、查询结果属性映射的最佳实践

## 使用驼峰映射结果属性

**(约定大于配置的最佳实践)**

Mybatis给我们提供了一种映射方式，如果属性的命名是遵从驼峰命名法的，数据列名遵从下划线命名。这样就可以一劳永逸，无论以后写多少查询SQL都不需要单独制定映射规则。 那么可以使用这种方式，类似如下：

- 实体类属性userName对应SQL的字段user_name;
- 实体类属性userId对应SQL的字段user_id;

在Spring boot环境下只需要写这样一个配置即可。

```yaml
mybatis:
    configuration:
      mapUnderscoreToCamelCase: true
```

## 其他的实现方式

**(知道有这几种方式即可，一旦别人用你知道怎么回事，自己不要用)**

其他的实现方式都很不友好，都需要写一个查询SQL，做一套映射配置。

### 第一种：xml的属性映射举例resultMap

```xml
<mapper namespace="data.UserMapper">
        <resultMap type="data.User" id="userResultMap">
                <!-- 用id属性来映射主键字段 -->
                <id property="id" column="user_id"/>
                <!-- 用result属性来映射非主键字段 -->
                <result property="userName" column="user_name"/>
        </resultMap>
</mapper>
```

### 第二种：通过注解 @Results 和 @Result
这两个注解是与XML文件中的标签相对应的：

- @Results对应resultMap
- @Result对应result
  这两个注解是应用在方法的级别上的，也就是在mapper方法上，如下：

```java
@Select("select * from t_user where user_name = #{userName}")
@Results(
        @Result(property = "userId", column = "user_id"),
        @Result(property = "userName", column = "user_name")
)
User getUserByName(@Param("userName") String userName);
```

### 第三种：通过SQL字段别名来完成映射

```java
@Select("select user_name as userName, user_id as userId from t_user where user_name = #{userName}")
User getUserByName(@Param("userName") String userName);
```

# 三、使用@MapperScan而不是@mapper

```java
@SpringBootApplication
@MapperScan(basePackages = {"club.krislin.**.mapper"})
public class DemoMybatisApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoMybatisApplication.class, args);
    }
}
```

这样就会自动扫描club.krislin.**.mapper目录下面的所有XXXXMapper文件，并且完成自动注入。
不需要在每一个Mapper上面都加@Mapper注解。
如下,不需要这么做：

```java
@Mapper  
public interface DemoMapper {  
    @Insert("insert into Demo(name) values(#{name})")  
    @Options(keyProperty="id",keyColumn="id",useGeneratedKeys=true)  
    public void save(Demo demo);  
}  
```

# 四、 将XxxxMapper.java文件和XxxxMapper.xml文件放在同一个目录下面

我们写代码的时候，通常是XxxxMapper.java文件和XxxxMapper.xml文件一起写的，新建和修改几乎都是一起进行。
但是按照springboot和maven的约定，java文件放在/src/main/java下面，xml文件要放在resources目录下面。
这样我们就要来回的切换目录，找文件，很麻烦。可以通过如下pom.xml配置来解决这个问题。通过如下配置，我们就可以将XML文件和java文件都放在/src/main/java下面子目录，在一起。

```xml
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <filtering>false</filtering>
            <includes>
                <include>**/*.xml</include>
            </includes>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <filtering>false</filtering>
        </resource>
    </resources>
```

# 五、使用PageHelper分页插件

引入maven依赖包

```xml
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.2.10</version>
        </dependency>
```

测试用例

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class MybatisTest {

    @Resource
    ArticleMapper articleMapper;

    @Test
    public void testPageHelper(){
        // 只有紧跟在PageHelper.startPage方法后的第一个Mybatis的查询（Select）方法会被分页!!!!
        PageHelper.startPage(1, 2);
        List<Article> articles = articleMapper.selectByExample(null);
        PageInfo<Article> page = PageInfo.of(articles);
        System.out.println(page);
    }
}
```