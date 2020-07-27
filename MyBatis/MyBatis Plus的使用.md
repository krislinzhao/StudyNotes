# 1.Mybatis-Plus

## 1.1 简介

MyBatis-Plus 是一个 Mybatis 增强版工具，在 MyBatis 上扩充了其他功能没有改变其基本功能，为了简化开发提交效率而存在。

官网文档地址：
　　https://mp.baomidou.com/guide/

MyBatis-Plus 特性：
　　https://mp.baomidou.com/guide/#%E7%89%B9%E6%80%A7

## 1.2 使用 SpringBoot 快速使用 MyBatis-Plus

1. 创建一个 SpringBoot 项目。
   　方式一：去官网 https://start.spring.io/ 初始化一个，然后导入 IDE 工具即可。
   　方式二：直接使用 IDE 工具创建一个。 Spring Initializer。

2. 添加 MyBatis-Plus 依赖（mybatis-plus-boot-starter）

   ```xml
   <dependency>
       <groupId>com.baomidou</groupId>
       <artifactId>mybatis-plus-boot-starter</artifactId>
       <version>3.3.1.tmp</version>
   </dependency>
   ```

3. 为了测试开发，此处使用 mysql 8，需要引入 mysql 相关依赖。为了简化代码，引入 lombok 依赖（减少 getter、setter 等方法）。

   ```xml
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>8.0.18</version>
   </dependency>
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.10</version>
   </dependency>
   ```

4. 使用一个表进行测试。仅供参考，可以定义 创建时间、修改时间等字段。

   ```sql
   DROP DATABASE IF EXISTS testMyBatisPlus;
   
   CREATE DATABASE testMyBatisPlus;
   
   USE testMyBatisPlus;
   
   DROP TABLE IF EXISTS user;
   
   CREATE TABLE user
   (
       id BIGINT(20) NOT NULL COMMENT '主键ID',
       name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
       age INT(11) NULL DEFAULT NULL COMMENT '年龄',
       email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
       PRIMARY KEY (id)
   );
   
   INSERT INTO user (id, name, age, email) VALUES
   (1, 'Jone', 18, 'test1@baomidou.com'),
   (2, 'Jack', 20, 'test2@baomidou.com'),
   (3, 'Tom', 28, 'test3@baomidou.com'),
   (4, 'Sandy', 21, 'test4@baomidou.com'),
   (5, 'Billie', 24, 'test5@baomidou.com');
   ```

5. 在 application.yml 文件中配置 mysql 数据源信息。

   ```yaml
   spring:
     datasource:
       driver-class-name: com.mysql.cj.jdbc.Driver
       username: root
       password: 123456
       url: jdbc:mysql://localhost:3306/testMyBatisPlus?useUnicode=true&characterEncoding=utf8
   ```

6. 编写表对应的实体类。

   ```java
   package entity;
   
   import lombok.Data;
   
   @Data
   public class User {
       private Long id;
       private String name;
       private int age;
       private String email;
   }
   ```

7. 编写操作实体类的 Mapper 类。直接继承 BaseMapper，这是 mybatis-plus 封装好的类。

   ```java
   package mapper;
   
   import bean.User;
   import com.baomidou.mybatisplus.core.mapper.BaseMapper;
   
   public interface UserMapper extends BaseMapper<User> {
   }
   ```

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727172032.png)

8. 实体类、Mapper 类都写好了，就可以使用了。
   Step1：先得在启动类里扫描 Mapper 类，即添加 @MapperScan 注解

   ```java
   import org.mybatis.spring.annotation.MapperScan;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   @MapperScan("mapper")
   @SpringBootApplication
   public class TestMybatisPlusApplication {
   
       public static void main(String[] args) {
           SpringApplication.run(TestMybatisPlusApplication.class, args);
       }
   
   }
   ```

   Step2：写一个测试类测试一下。

   ```java
   import bean.User;
   import mapper.UserMapper;
   import org.junit.jupiter.api.Test;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.test.context.SpringBootTest;
   
   import java.util.List;
   
   @SpringBootTest
   class TestMybatisPlusApplicationTests {
   
       @Autowired
       private UserMapper userMapper;
   
       @Test
       public void testSelect() {
           System.out.println(("----- selectAll method test ------"));
           List<User> userList = userMapper.selectList(null);
           for(User user:userList) {
               System.out.println(user);
           }
       }
   }
   ```

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727172310.png)

9. 总结：通过以上简单操作，就能对 user 表进行 CRUD 操作，不需要去编写 xml 文件。
   注：若遇到 @Autowired 标记的变量出现 红色下划线，但是不影响 正常运行。

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727172450.png)

   可以进入 Settings，找到 Inspection，并选择其中的 Spring Core -> Code -> Autowiring for Bean Class,将 Error 改为 Warning，即可。

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727172542.png)

# 2.Mybatis-Plus 常用操作

## 2.1 配置日志(两种方式)

### 1.注解方式，yml文件配置上以下就可以直接使用

```yaml
mybatis-plus:
  mapper-locations: classpath:mapper/*.xml
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

### 2.在sqlSessionFactory中配置

```java
@Bean("sqlSessionFactory")
public SqlSessionFactory sqlSessionFactory() throws Exception {
    // 导入mybatissqlsession配置
    MybatisSqlSessionFactoryBean sessionFactory = new MybatisSqlSessionFactoryBean();
    // 指明数据源
    sessionFactory.setDataSource(multipleDataSource(dataSource0(), dataSource1(), dataSource2()));
    // 指明mapper.xml位置(配置文件中指明的xml位置会失效用此方式代替，具体原因未知)
    sessionFactory.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath*:/mapper/**Mapper.xml"));
    // 指明实体扫描(多个package用逗号或者分号分隔)
    sessionFactory.setTypeAliasesPackage("cn.krislin.entity");
    // 导入mybatis配置
    MybatisConfiguration configuration = new MybatisConfiguration();
    configuration.setJdbcTypeForNull(JdbcType.NULL);
    configuration.setMapUnderscoreToCamelCase(true);
    configuration.setCacheEnabled(false);
    // 配置打印sql语句
    configuration.setLogImpl(StdOutImpl.class);
    sessionFactory.setConfiguration(configuration);
    // 添加分页功能
    sessionFactory.setPlugins(new Interceptor[]{
        paginationInterceptor()
    });
    // 导入全局配置
    sessionFactory.setGlobalConfig(globalConfiguration());
    return sessionFactory.getObject();
}
```

主要就是这句

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727173327.png)

点击setLogImpl看源码，找到Configuration()构造方法，就可以看见了

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727173359.png)

## 2.2 常用注解

```
【@TableName 】
    @TableName               用于定义表名
注：
    常用属性：
        value                用于定义表名

【@TableId】
    @TableId                 用于定义表的主键
注：
    常用属性：
        value           用于定义主键字段名
        type            用于定义主键类型（主键策略 IdType）

   主键策略：
      IdType.AUTO          主键自增，系统分配，不需要手动输入
      IdType.NONE          未设置主键
      IdType.INPUT         需要自己输入 主键值。
      IdType.ASSIGN_ID     系统分配 ID，用于数值型数据（Long，对应 mysql 中 BIGINT 类型）。
      IdType.ASSIGN_UUID   系统分配 UUID，用于字符串型数据（String，对应 mysql 中 varchar(32) 类型）。

【@TableField】  
    @TableField            用于定义表的非主键字段。
注：
    常用属性：
        value                用于定义非主键字段名
        exist                用于指明是否为数据表的字段， true 表示是，false 为不是。
        fill                 用于指定字段填充策略（FieldFill）。
        
    字段填充策略：（一般用于填充 创建时间、修改时间等字段）
        FieldFill.DEFAULT         默认不填充
        FieldFill.INSERT          插入时填充
        FieldFill.UPDATE          更新时填充
        FieldFill.INSERT_UPDATE   插入、更新时填充。

【@TableLogic】
    @TableLogic           用于定义表的字段进行逻辑删除（非物理删除）
注：
    常用属性：
        value            用于定义未删除时字段的值
        delval           用于定义删除时字段的值
        
【@Version】
    @Version             用于字段实现乐观锁
```

## 2.3 代码生成器

1. AutoGenerator 简介

   AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、Mapper XML、Service、Controller 等各个模块的代码，极大的提升了开发效率。
   与 mybatis 中的 mybatis-generator-core 类似。

2. 添加依赖

   ```xml
   <dependency>
       <groupId>com.baomidou</groupId>
       <artifactId>mybatis-plus-generator</artifactId>
       <version>3.3.1.tmp</version>
   </dependency>
   <!-- 添加 模板引擎 依赖 -->
   <dependency>
       <groupId>org.apache.velocity</groupId>
       <artifactId>velocity-engine-core</artifactId>
       <version>2.2</version>
   </dependency>
   ```

3. 代码分析
   Step1：
   　创建一个 代码生成器。用于生成代码。
   　此处不用修改。

   ```java
   // Step1：代码生成器
   AutoGenerator mpg = new AutoGenerator();
   ```

   Step2：
   　　配置全局信息。指定代码输出路径，以及包名、作者等信息。
   　　此处按需添加，projectPath 需要修改，setAuthor 需要修改。

   ```java
   // Step2：全局配置
   GlobalConfig gc = new GlobalConfig();
   // 填写代码生成的目录(需要修改)
   String projectPath = "E:\\myProject\\test\\test_mybatis_plus";
   // 拼接出代码最终输出的目录
   gc.setOutputDir(projectPath + "/src/main/java");
   // 配置开发者信息（可选）（需要修改）
   gc.setAuthor("krislin");
   // 配置是否打开目录，false 为不打开（可选）
   gc.setOpen(false);
   // 实体属性 Swagger2 注解，添加 Swagger 依赖，开启 Swagger2 模式（可选）
   //gc.setSwagger2(true);
   // 重新生成文件时是否覆盖，false 表示不覆盖（可选）
   gc.setFileOverride(false);
   // 配置主键生成策略，此处为 ASSIGN_ID（可选）
   gc.setIdType(IdType.ASSIGN_ID);
   // 配置日期类型，此处为 ONLY_DATE（可选）
   gc.setDateType(DateType.ONLY_DATE);
   // 默认生成的 service 会有 I 前缀
   gc.setServiceName("%sService");
   mpg.setGlobalConfig(gc);
   ```

   Step3：
   　　配置数据源信息。用于指定 需要生成代码的 数据仓库、数据表。
   　　setUrl、setDriverName、setUsername、setPassword 均需修改。

   ```java
   // Step3：数据源配置（需要修改）
   DataSourceConfig dsc = new DataSourceConfig();
   // 配置数据库 url 地址
   dsc.setUrl("jdbc:mysql://localhost:3306/testMyBatisPlus?useUnicode=true&characterEncoding=utf8");
   // dsc.setSchemaName("testMyBatisPlus"); // 可以直接在 url 中指定数据库名
   // 配置数据库驱动
   dsc.setDriverName("com.mysql.cj.jdbc.Driver");
   // 配置数据库连接用户名
   dsc.setUsername("root");
   // 配置数据库连接密码
   dsc.setPassword("123456");
   mpg.setDataSource(dsc);
   ```

   Step4：
   　　配置包信息。
   　　setParent、setModuleName 均需修改。其余按需求修改.

   ```java
   // Step:4：包配置
   PackageConfig pc = new PackageConfig();
   // 配置父包名（需要修改）
   pc.setParent("com.krislin.test");
   // 配置模块名（需要修改）
   pc.setModuleName("test_mybatis_plus");
   // 配置 entity 包名
   pc.setEntity("entity");
   // 配置 mapper 包名
   pc.setMapper("mapper");
   // 配置 service 包名
   pc.setService("service");
   // 配置 controller 包名
   pc.setController("controller");
   mpg.setPackageInfo(pc);
   ```

   Step5：
   　　配置数据表映射信息。
   　　setInclude 需要修改，其余按实际开发修改。

   ```java
   // Step5：策略配置（数据库表配置）
   StrategyConfig strategy = new StrategyConfig();
   // 指定表名（可以同时操作多个表，使用 , 隔开）（需要修改）
   strategy.setInclude("test_mybatis_plus_user");
   // 配置数据表与实体类名之间映射的策略
   strategy.setNaming(NamingStrategy.underline_to_camel);
   // 配置数据表的字段与实体类的属性名之间映射的策略
   strategy.setColumnNaming(NamingStrategy.underline_to_camel);
   // 配置 lombok 模式
   strategy.setEntityLombokModel(true);
   // 配置 rest 风格的控制器（@RestController）
   strategy.setRestControllerStyle(true);
   // 配置驼峰转连字符
   strategy.setControllerMappingHyphenStyle(true);
   // 配置表前缀，生成实体时去除表前缀
   // 此处的表名为 test_mybatis_plus_user，模块名为 test_mybatis_plus，去除前缀后剩下为 user。
   strategy.setTablePrefix(pc.getModuleName() + "_");
   mpg.setStrategy(strategy);
   ```

   Step6：
   　　执行代码生成操作。

   　　此处不用修改。

   ```java
   // Step6：执行代码生成操作
   mpg.execute();
   ```

完整配置如下：

```java
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.rules.DateType;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
import org.junit.jupiter.api.Test;

public class TestAutoGenerate {
    @Test
    public void autoGenerate() {
        // Step1：代码生成器
        AutoGenerator mpg = new AutoGenerator();

        // Step2：全局配置
        GlobalConfig gc = new GlobalConfig();
        // 填写代码生成的目录(需要修改)
        String projectPath = "E:\\myProject\\test\\test_mybatis_plus";
        // 拼接出代码最终输出的目录
        gc.setOutputDir(projectPath + "/src/main/java");
        // 配置开发者信息（可选）（需要修改）
        gc.setAuthor("krislin");
        // 配置是否打开目录，false 为不打开（可选）
        gc.setOpen(false);
        // 实体属性 Swagger2 注解，添加 Swagger 依赖，开启 Swagger2 模式（可选）
        //gc.setSwagger2(true);
        // 重新生成文件时是否覆盖，false 表示不覆盖（可选）
        gc.setFileOverride(false);
        // 配置主键生成策略，此处为 ASSIGN_ID（可选）
        gc.setIdType(IdType.ASSIGN_ID);
        // 配置日期类型，此处为 ONLY_DATE（可选）
        gc.setDateType(DateType.ONLY_DATE);
        // 默认生成的 service 会有 I 前缀
        gc.setServiceName("%sService");
        mpg.setGlobalConfig(gc);

        // Step3：数据源配置（需要修改）
        DataSourceConfig dsc = new DataSourceConfig();
        // 配置数据库 url 地址
        dsc.setUrl("jdbc:mysql://localhost:3306/testMyBatisPlus?useUnicode=true&characterEncoding=utf8");
        // dsc.setSchemaName("testMyBatisPlus"); // 可以直接在 url 中指定数据库名
        // 配置数据库驱动
        dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        // 配置数据库连接用户名
        dsc.setUsername("root");
        // 配置数据库连接密码
        dsc.setPassword("123456");
        mpg.setDataSource(dsc);

        // Step:4：包配置
        PackageConfig pc = new PackageConfig();
        // 配置父包名（需要修改）
        pc.setParent("com.krislin.test");
        // 配置模块名（需要修改）
        pc.setModuleName("test_mybatis_plus");
        // 配置 entity 包名
        pc.setEntity("entity");
        // 配置 mapper 包名
        pc.setMapper("mapper");
        // 配置 service 包名
        pc.setService("service");
        // 配置 controller 包名
        pc.setController("controller");
        mpg.setPackageInfo(pc);

        // Step5：策略配置（数据库表配置）
        StrategyConfig strategy = new StrategyConfig();
        // 指定表名（可以同时操作多个表，使用 , 隔开）（需要修改）
        strategy.setInclude("test_mybatis_plus_user");
        // 配置数据表与实体类名之间映射的策略
        strategy.setNaming(NamingStrategy.underline_to_camel);
        // 配置数据表的字段与实体类的属性名之间映射的策略
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        // 配置 lombok 模式
        strategy.setEntityLombokModel(true);
        // 配置 rest 风格的控制器（@RestController）
        strategy.setRestControllerStyle(true);
        // 配置驼峰转连字符
        strategy.setControllerMappingHyphenStyle(true);
        // 配置表前缀，生成实体时去除表前缀
        // 此处的表名为 test_mybatis_plus_user，模块名为 test_mybatis_plus，去除前缀后剩下为 user。
        strategy.setTablePrefix(pc.getModuleName() + "_");
        mpg.setStrategy(strategy);

        // Step6：执行代码生成操作
        mpg.execute();
    }
}
```

## 2.4 自动填充数据功能

1. 简介
   添加、修改数据时，每次都会使用相同的方式进行填充。比如 数据的创建时间、修改时间等。Mybatis-plus 支持自动填充这些字段的数据。

   给之前的数据表新增两个字段：创建时间、修改时间。

   ```sql
   CREATE TABLE test_mybatis_plus_user
   (
       id BIGINT NOT NULL COMMENT '主键ID',
       name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
       age INT(11) NULL DEFAULT NULL COMMENT '年龄',
       email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
       create_time timestamp NULL DEFAULT NULL COMMENT '创建时间',
       update_time timestamp NULL DEFAULT NULL COMMENT '最后修改时间', 
       PRIMARY KEY (id)
   );
   ```

   并使用 代码生成器生成代码。

2. 未使用自动填充时

   未使用 自动填充时，每次添加、修改数据都可以手动对其进行添加。

   ```java
   @SpringBootTest
   class TestMybatisPlusApplicationTests {
   
       @Autowired
       private UserService userService;
   
       @Test
       public void testUpdate() {
           User user = new User();
           user.setName("tom").setAge(20).setEmail("tom@163.com");
           // 手动添加数据
           user.setCreateTime(new Date()).setUpdateTime(new Date());
           if (userService.save(user)) {
               userService.list().forEach(System.out::println);
           } else {
               System.out.println("添加数据失败");
           }
       }
   }
   ```

3. 使用自动填充功能。
   Step1：
   　　使用 @TableField 注解，标注需要进行填充的字段。

   ```java
   /**
    * 创建时间
    */
   @TableField(fill = FieldFill.INSERT)
   private Date createTime;
   
   /**
    * 最后修改时间
    */
   @TableField(fill = FieldFill.INSERT_UPDATE)
   private Date updateTime;
   ```

   Step2：
   　　自定义一个类，实现 MetaObjectHandler 接口，并重写方法。
   　　添加 @Component 注解，交给 Spring 去管理。

   ```java
   import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
   import org.apache.ibatis.reflection.MetaObject;
   import org.springframework.stereotype.Component;
   
   import java.util.Date;
   
   @Component
   public class MyMetaObjectHandler implements MetaObjectHandler {
       @Override
       public void insertFill(MetaObject metaObject) {
           this.strictInsertFill(metaObject, "createTime", Date.class, new Date());
           this.strictInsertFill(metaObject, "updateTime", Date.class, new Date());
       }
   
       @Override
       public void updateFill(MetaObject metaObject) {
           this.strictUpdateFill(metaObject, "updateTime", Date.class, new Date());
       }
   }
   ```

   Step3：
   　　简单测试一下。

   ```java
   @Test
   public void testAutoFill() {
       User user = new User();
       user.setName("tom").setAge(20).setEmail("tom@163.com");
       if (userService.save(user)) {
           userService.list().forEach(System.out::println);
       } else {
           System.out.println("添加数据失败");
       }
   }
   ```

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727174931.png)

## 2.5 逻辑删除

1. 简介
   　删除数据，可以通过物理删除，也可以通过逻辑删除。
   　物理删除指的是直接将数据从数据库中删除，不保留。
   　逻辑删除指的是修改数据的某个字段，使其表示为已删除状态，而非删除数据，保留该数据在数据库中，但是查询时不显示该数据（查询时过滤掉该数据）。

   　给数据表增加一个字段：delete_flag，用于表示该数据是否被逻辑删除。

   ```sql
   CREATE TABLE test_mybatis_plus_user
   (
       id BIGINT NOT NULL COMMENT '主键ID',
       name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
       age INT(11) NULL DEFAULT NULL COMMENT '年龄',
       email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
       create_time timestamp NULL DEFAULT NULL COMMENT '创建时间',
       update_time timestamp NULL DEFAULT NULL COMMENT '最后修改时间', 
       delete_flag tinyint(1) NULL DEFAULT NULL COMMENT '逻辑删除（0 未删除、1 删除）',
       PRIMARY KEY (id)
   );
   ```

2. 使用逻辑删除。
   可以定义一个自动填充规则，初始值为 0。0 表示未删除， 1 表示删除。

   ```java
   /**
    * 逻辑删除（0 未删除、1 删除）
    */
   @TableLogic(value = "0", delval = "1")
   @TableField(fill = FieldFill.INSERT)
   private Integer deleteFlag;
   
   
   @Override
   public void insertFill(MetaObject metaObject) {
       this.strictInsertFill(metaObject, "deleteFlag", Integer.class, 0);
   }
   ```

3. 简单测试
   使用 mybatis-plus 封装好的方法时，会自动添加逻辑删除的功能。
   若是自定义的 sql 语句，需要手动添加逻辑。

   ```java
   @Test
   public void testDelete() {
       if (userService.removeById(1258924257048547329L)) {
           System.out.println("删除数据成功");
           userService.list().forEach(System.out::println);
       } else {
           System.out.println("删除数据失败");
       }
   }
   ```

   现有数据 

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727175344.png)

   执行 testDelete 进行逻辑删除。

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727175434.png)

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727175506.png)

   若去除 TableLogic 注解，再执行 testDelete 时进行物理删除，直接删除这条数据。

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727175605.png)

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727175636.png)

## 2.6 分页插件的使用

1. 简介
   　　与 mybatis 的插件 pagehelper 用法类似。
   　　通过简单的配置即可使用。

2. 使用
   Step1：
   　　配置分页插件。
   　　编写一个 配置类，内部使用 @Bean 注解将 PaginationInterceptor 交给 Spring 容器管理。

   ```java
   import com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor;
   import org.mybatis.spring.annotation.MapperScan;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   /**
    * 自定义一个配置类，mapper 扫描也可在此写上
    */
   @Configuration
   @MapperScan("com.krislin.test.test_mybatis_plus.mapper")
   public class Myconfig {
       /**
        * 分页插件
        * @return 分页插件的实例
        */
       @Bean
       public PaginationInterceptor paginationInterceptor() {
           return new PaginationInterceptor();
       }
   }
   ```

   Step2：
   　　编写分页代码。
   　　直接 new 一个 Page 对象，对象需要传递两个参数（当前页，每页显示的条数）。
   　　调用 mybatis-plus 提供的分页查询方法，其会将 分页查询的数据封装到 Page 对象中。

   ```java
   @Test
   public void testPage() {
       // Step1：创建一个 Page 对象
       Page<User> page = new Page<>();
       // Page<User> page = new Page<>(2, 5);
       // Step2：调用 mybatis-plus 提供的分页查询方法
       userService.page(page, null);
       // Step3：获取分页数据
       System.out.println(page.getCurrent()); // 获取当前页
       System.out.println(page.getTotal()); // 获取总记录数
       System.out.println(page.getSize()); // 获取每页的条数
       System.out.println(page.getRecords()); // 获取每页数据的集合
       System.out.println(page.getPages()); // 获取总页数
       System.out.println(page.hasNext()); // 是否存在下一页
       System.out.println(page.hasPrevious()); // 是否存在上一页
   }
   ```

## 2.7 乐观锁的实现

1. 首先认识一下 读问题、写问题？
   操作数据库数据时，遇到的最基本问题就是 读问题与写问题。
   读问题 指的是从数据库中读取数据时遇到的问题，比如：脏读、幻读、不可重复读。

   写问题 指的是数据写入数据库时遇到的问题，比如：丢失更新（多个线程同时对某条数据更新，无论执行顺序如何，都会丢失其他线程更新的数据）

2. 如何解决写问题？
   乐观锁、悲观锁就是为了解决 写问题而存在的。

   乐观锁：总是假设最好的情况，每次读取数据时认为数据不会被修改（即不加锁），当进行更新操作时，会判断这条数据是否被修改，未被修改，则进行更新操作。若被修改，则数据更新失败，可以对数据进行重试（重新尝试修改数据）。
   悲观锁：总是假设最坏的情况，每次读取数据时认为数据会被修改（即加锁），当进行更新操作时，直接更新数据，结束操作后释放锁（此处才可以被其他线程读取）。

3. 乐观锁、悲观锁使用场景？
   乐观锁一般用于读比较多的场合，尽量减少加锁的开销。
   悲观锁一般用于写比较多的场合，尽量减少 类似 乐观锁重试更新引起的性能开销。

4. 乐观锁两种实现方式
   方式一：通过版本号机制实现。
    在数据表中增加一个 version 字段。
    取数据时，获取该字段，更新时以该字段为条件进行处理（即set version = new Version where version = oldVersion），若 version 相同，则更新成功（给新 version 赋一个值，一般加 1）。若 version 不同，则更新失败，可以重新尝试更新操作。

   

   方式二：通过 CAS 算法实现。
   CAS 为 Compare And Swap 的缩写，即比较交换，是一种无锁算法（即在不加锁的情况实现多线程之间的变量同步）。
   CAS 操作包含三个操作数 —— 内存值（V）、预期原值（A）和新值(B)。如果内存地址里面的值 V 和 A 的值是一样的，那么就将内存里面的值更新成B。若 V 与 A 不一致，则不执行任何操作（可以通过自旋操作，不断尝试修改数据直至成功修改）。即 V == A ？ V = B ： V = V。
   CAS 可能导致 ABA 问题（两次读取数据时值相同，但不确定值是否被修改过），比如两个线程操作同一个变量，线程 A、线程B 初始读取数据均为 A，后来 线程B 将数据修改为 B，然后又修改为 A，此时线程 A 再次读取到的数据依旧是 A，虽然值相同但是中间被修改过，这就是 ABA 问题。可以加一个额外的标志位 C，用于表示数据是否被改。当标志位 C 与预期标志位相同、且 V == A 时，则更新值 B。

5. mybatis-plus 实现乐观锁（通过 version 机制）
   实现思路：
   　　Step1：取出记录时，获取当前version
   　　Step2：更新时，带上这个version
   　　Step3：执行更新时， set version = new Version where version = oldVersion
   　　Step4：如果version不对，就更新失败

6. mybatis-plus 代码实现乐观锁
   Step1：
   　　配置乐观锁插件。
   　　编写一个配置类（可以与上例的分页插件共用一个配置类），将 OptimisticLockerInterceptor 通过 @Bean 交给 Spring 管理。

   ```java
   /**
    * 乐观锁插件
    * @return 乐观锁插件的实例
    */
   @Bean
   public OptimisticLockerInterceptor optimisticLockerInterceptor() {
       return new OptimisticLockerInterceptor();
   }
   ```

   Step2：
   　　定义一个数据库字段 version。

   ```sql
   CREATE TABLE test_mybatis_plus_user
   (
       id BIGINT NOT NULL COMMENT '主键ID',
       name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
       age INT(11) NULL DEFAULT NULL COMMENT '年龄',
       email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
       create_time timestamp NULL DEFAULT NULL COMMENT '创建时间',
       update_time timestamp NULL DEFAULT NULL COMMENT '最后修改时间', 
       delete_flag tinyint(1) NULL DEFAULT NULL COMMENT '逻辑删除（0 未删除、1 删除）',
       version int NULL DEFAULT NULL COMMENT '版本号（用于乐观锁， 默认为 1）',
       PRIMARY KEY (id)
   );
   ```

   Step3：
   　　使用 @Version 注解标注对应的实体类。
   　　可以通过 @TableField 进行数据自动填充。

   ```java
   /**
    * 版本号（用于乐观锁， 默认为 1）
    */
   @Version
   @TableField(fill = FieldFill.INSERT)
   private Integer version;
   
   
   @Override
   public void insertFill(MetaObject metaObject) {
       this.strictInsertFill(metaObject, "version", Integer.class, 1);
   }
   ```

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727181316.png)

   Step4：
   　　简单测试一下，可以看一下 控制台 打印的 sql 语句。

   ```java
   @Test
   public void testVersion() {
       User user = new User();
       user.setName("tom").setAge(20).setEmail("tom@163.com");
       userService.save(user);
       userService.list().forEach(System.out::println);
       user.setName("jarry");
       userService.update(user, null);
       userService.list().forEach(System.out::println);
   }
   ```

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727181435.png)

# 3.Mybatis-Plus CRUD 操作

## 3.1 Mapper 接口方法（CRUD）

1. 简介
   使用代码生成器生成的 mapper 接口中，其继承了 BaseMapper 接口。
   而 BaseMapper 接口中封装了一系列 CRUD 常用操作，可以直接使用，而不用自定义 xml 与 sql 语句进行 CRUD 操作（当然根据实际开发需要，自定义 sql 还是有必要的）。

   此处简单介绍一下 BaseMapper 接口中的常用方法。

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727181720.png)

2. 方法介绍

   ```
   【添加数据：（增）】
       int insert(T entity);              // 插入一条记录
   注：
       T         表示任意实体类型
       entity    表示实体对象
   
   【删除数据：（删）】
       int deleteById(Serializable id);    // 根据主键 ID 删除
       int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);  // 根据 map 定义字段的条件删除
       int delete(@Param(Constants.WRAPPER) Wrapper<T> wrapper); // 根据实体类定义的 条件删除对象
       int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList); // 进行批量删除
   注：
       id        表示 主键 ID
       columnMap 表示表字段的 map 对象
       wrapper   表示实体对象封装操作类，可以为 null。
       idList    表示 主键 ID 集合（列表、数组），不能为 null 或 empty
   
   【修改数据：（改）】
       int updateById(@Param(Constants.ENTITY) T entity); // 根据 ID 修改实体对象。
       int update(@Param(Constants.ENTITY) T entity, @Param(Constants.WRAPPER) Wrapper<T> updateWrapper); // 根据 updateWrapper 条件修改实体对象
   注：
       update 中的 entity 为 set 条件，可以为 null。
       updateWrapper 表示实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句）
   
   【查询数据：（查）】
       T selectById(Serializable id); // 根据 主键 ID 查询数据
       List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList); // 进行批量查询
       List<T> selectByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap); // 根据表字段条件查询
       T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper); // 根据实体类封装对象 查询一条记录
       Integer selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper); // 查询记录的总条数
       List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper); // 查询所有记录（返回 entity 集合）
       List<Map<String, Object>> selectMaps(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper); // 查询所有记录（返回 map 集合）
       List<Object> selectObjs(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper); // 查询所有记录（但只保存第一个字段的值）
       <E extends IPage<T>> E selectPage(E page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper); // 查询所有记录（返回 entity 集合），分页
       <E extends IPage<Map<String, Object>>> E selectMapsPage(E page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper); // 查询所有记录（返回 map 集合），分页
   注：
       queryWrapper 表示实体对象封装操作类（可以为 null）
       page 表示分页查询条件
   ```

## 3.2 Service 接口方法（CRUD）

1. 简介
   使用 代码生成器 生成的 service 接口中，其继承了 IService 接口。
   IService 内部进一步封装了 BaseMapper 接口的方法（当然也提供了更详细的方法）。
   使用时，可以通过 生成的 mapper 类进行 CRUD 操作，也可以通过 生成的 service 的实现类进行 CRUD 操作。（当然，自定义代码执行也可）
   此处简单介绍一下 IService 中封装的常用方法。

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727182008.png)

2. 方法介绍
   内部封装了 BaseMapper 的方法，也提供了新的方法。
   比如：
   　　添加了 批量更新 方法、更新或修改方法等。
   　　对 查询方法 做了细化，使用 get 命名的方法查询一条数据，使用 list 命名的方法查询多条数据等。
   　　增加了链式调用的方法。

   ```
   【添加数据：（增）】
       default boolean save(T entity); // 调用 BaseMapper 的 insert 方法，用于添加一条数据。
       boolean saveBatch(Collection<T> entityList, int batchSize); // 批量插入数据
   注：
       entityList 表示实体对象集合 
       batchSize 表示一次批量插入的数据量，默认为 1000
   
   【添加或修改数据：（增或改）】
       boolean saveOrUpdate(T entity); // id 若存在，则修改， id 不存在则新增数据
      default boolean saveOrUpdate(T entity, Wrapper<T> updateWrapper); // 先根据条件尝试更新，然后再执行 saveOrUpdate 操作
      boolean saveOrUpdateBatch(Collection<T> entityList, int batchSize); // 批量插入并修改数据 
   
   【删除数据：（删）】
       default boolean removeById(Serializable id); // 调用 BaseMapper 的 deleteById 方法，根据 id 删除数据。
       default boolean removeByMap(Map<String, Object> columnMap); // 调用 BaseMapper 的 deleteByMap 方法，根据 map 定义字段的条件删除
       default boolean remove(Wrapper<T> queryWrapper); // 调用 BaseMapper 的 delete 方法，根据实体类定义的 条件删除对象。
       default boolean removeByIds(Collection<? extends Serializable> idList); // 用 BaseMapper 的 deleteBatchIds 方法, 进行批量删除。
       
   【修改数据：（改）】
       default boolean updateById(T entity); // 调用 BaseMapper 的 updateById 方法，根据 ID 选择修改。
       default boolean update(T entity, Wrapper<T> updateWrapper); // 调用 BaseMapper 的 update 方法，根据 updateWrapper 条件修改实体对象。
       boolean updateBatchById(Collection<T> entityList, int batchSize); // 批量更新数据
   
   【查找数据：（查）】
       default T getById(Serializable id); // 调用 BaseMapper 的 selectById 方法，根据 主键 ID 返回数据。
       default List<T> listByIds(Collection<? extends Serializable> idList); // 调用 BaseMapper 的 selectBatchIds 方法，批量查询数据。
       default List<T> listByMap(Map<String, Object> columnMap); // 调用 BaseMapper 的 selectByMap 方法，根据表字段条件查询
       default T getOne(Wrapper<T> queryWrapper); // 返回一条记录（实体类保存）。
       Map<String, Object> getMap(Wrapper<T> queryWrapper); // 返回一条记录（map 保存）。
       default int count(Wrapper<T> queryWrapper); // 根据条件返回 记录数。
       default List<T> list(); // 返回所有数据。
       default List<T> list(Wrapper<T> queryWrapper); // 调用 BaseMapper 的 selectList 方法，查询所有记录（返回 entity 集合）。
       default List<Map<String, Object>> listMaps(Wrapper<T> queryWrapper); // 调用 BaseMapper 的 selectMaps 方法，查询所有记录（返回 map 集合）。
       default List<Object> listObjs(); // 返回全部记录，但只返回第一个字段的值。
       default <E extends IPage<T>> E page(E page, Wrapper<T> queryWrapper); // 调用 BaseMapper 的 selectPage 方法，分页查询
       default <E extends IPage<Map<String, Object>>> E pageMaps(E page, Wrapper<T> queryWrapper); // 调用 BaseMapper 的 selectMapsPage 方法，分页查询
   注：
       get 用于返回一条记录。
       list 用于返回多条记录。
       count 用于返回记录总数。
       page 用于分页查询。
       
   【链式调用：】
       default QueryChainWrapper<T> query(); // 普通链式查询
       default LambdaQueryChainWrapper<T> lambdaQuery(); // 支持 Lambda 表达式的修改
       default UpdateChainWrapper<T> update(); // 普通链式修改
       default LambdaUpdateChainWrapper<T> lambdaUpdate(); // 支持 Lambda 表达式的修改
   注：
       query 表示查询
       update 表示修改
       Lambda 表示内部支持 Lambda 写法。
   形如：
       query().eq("column", value).one();
       lambdaQuery().eq(Entity::getId, value).list();
       update().eq("column", value).remove();
       lambdaUpdate().eq(Entity::getId, value).update(entity);
   ```

## 3.3 条件构造器（Wrapper，定义 where 条件）

1. 简介
   上面介绍的 接口方法的参数中，会出现各种 wrapper，比如 queryWrapper、updateWrapper 等。wrapper 的作用就是用于定义各种各样的查询条件（where）。

   ```
   Wrapper  条件构造抽象类
       -- AbstractWrapper 查询条件封装，用于生成 sql 中的 where 语句。
           -- QueryWrapper Entity 对象封装操作类，用于查询。
           -- UpdateWrapper Update 条件封装操作类，用于更新。
       -- AbstractLambdaWrapper 使用 Lambda 表达式封装 wrapper
           -- LambdaQueryWrapper 使用 Lambda 语法封装条件，用于查询。
           -- LambdaUpdateWrapper 使用 Lambda 语法封装条件，用于更新。
   ```

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727182500.png)

2. 常用条件

   ```
   【通用条件：】
   【比较大小： ( =, <>, >, >=, <, <= )】
       eq(R column, Object val); // 等价于 =，例: eq("name", "老王") ---> name = '老王'
       ne(R column, Object val); // 等价于 <>，例: ne("name", "老王") ---> name <> '老王'
       gt(R column, Object val); // 等价于 >，例: gt("name", "老王") ---> name > '老王'
       ge(R column, Object val); // 等价于 >=，例: ge("name", "老王") ---> name >= '老王'
       lt(R column, Object val); // 等价于 <，例: lt("name", "老王") ---> name < '老王'
       le(R column, Object val); // 等价于 <=，例: le("name", "老王") ---> name <= '老王'
       
   【范围：（between、not between、in、not in）】
      between(R column, Object val1, Object val2); // 等价于 between a and b, 例： between("age", 18, 30) ---> age between 18 and 30
      notBetween(R column, Object val1, Object val2); // 等价于 not between a and b, 例： notBetween("age", 18, 30) ---> age not between 18 and 30
      in(R column, Object... values); // 等价于 字段 IN (v0, v1, ...),例: in("age",{1,2,3}) ---> age in (1,2,3)
      notIn(R column, Object... values); // 等价于 字段 NOT IN (v0, v1, ...), 例: notIn("age",{1,2,3}) ---> age not in (1,2,3)
      inSql(R column, Object... values); // 等价于 字段 IN (sql 语句), 例: inSql("id", "select id from table where id < 3") ---> id in (select id from table where id < 3)
      notInSql(R column, Object... values); // 等价于 字段 NOT IN (sql 语句)
      
   【模糊匹配：（like）】
       like(R column, Object val); // 等价于 LIKE '%值%'，例: like("name", "王") ---> name like '%王%'
       notLike(R column, Object val); // 等价于 NOT LIKE '%值%'，例: notLike("name", "王") ---> name not like '%王%'
       likeLeft(R column, Object val); // 等价于 LIKE '%值'，例: likeLeft("name", "王") ---> name like '%王'
       likeRight(R column, Object val); // 等价于 LIKE '值%'，例: likeRight("name", "王") ---> name like '王%'
       
   【空值比较：（isNull、isNotNull）】
       isNull(R column); // 等价于 IS NULL，例: isNull("name") ---> name is null
       isNotNull(R column); // 等价于 IS NOT NULL，例: isNotNull("name") ---> name is not null
   
   【分组、排序：（group、having、order）】
       groupBy(R... columns); // 等价于 GROUP BY 字段, ...， 例: groupBy("id", "name") ---> group by id,name
       orderByAsc(R... columns); // 等价于 ORDER BY 字段, ... ASC， 例: orderByAsc("id", "name") ---> order by id ASC,name ASC
       orderByDesc(R... columns); // 等价于 ORDER BY 字段, ... DESC， 例: orderByDesc("id", "name") ---> order by id DESC,name DESC
       having(String sqlHaving, Object... params); // 等价于 HAVING ( sql语句 )， 例: having("sum(age) > {0}", 11) ---> having sum(age) > 11
   
   【拼接、嵌套 sql：（or、and、nested、apply）】
      or(); // 等价于 a or b， 例：eq("id",1).or().eq("name","老王") ---> id = 1 or name = '老王'
      or(Consumer<Param> consumer); // 等价于 or(a or/and b)，or 嵌套。例: or(i -> i.eq("name", "李白").ne("status", "活着")) ---> or (name = '李白' and status <> '活着')
      and(Consumer<Param> consumer); // 等价于 and(a or/and b)，and 嵌套。例: and(i -> i.eq("name", "李白").ne("status", "活着")) ---> and (name = '李白' and status <> '活着')
      nested(Consumer<Param> consumer); // 等价于 (a or/and b)，普通嵌套。例: nested(i -> i.eq("name", "李白").ne("status", "活着")) ---> (name = '李白' and status <> '活着')
      apply(String applySql, Object... params); // 拼接sql（若不使用 params 参数，可能存在 sql 注入），例: apply("date_format(dateColumn,'%Y-%m-%d') = {0}", "2008-08-08") ---> date_format(dateColumn,'%Y-%m-%d') = '2008-08-08'")
      last(String lastSql); // 无视优化规则直接拼接到 sql 的最后，可能存若在 sql 注入。
      exists(String existsSql); // 拼接 exists 语句。例: exists("select id from table where age = 1") ---> exists (select id from table where age = 1)
      
   【QueryWrapper 条件：】
       select(String... sqlSelect); // 用于定义需要返回的字段。例： select("id", "name", "age") ---> select id, name, age
       select(Predicate<TableFieldInfo> predicate); // Lambda 表达式，过滤需要的字段。
       lambda(); // 返回一个 LambdaQueryWrapper
       
   【UpdateWrapper 条件：】
       set(String column, Object val); // 用于设置 set 字段值。例: set("name", null) ---> set name = null
       etSql(String sql); // 用于设置 set 字段值。例: setSql("name = '老李头'") ---> set name = '老李头'
       lambda(); // 返回一个 LambdaUpdateWrapper
   ```

3. 简单使用，测试一下 QueryWrapper 

   ```java
   @Test
   public void testQueryWrapper() {
       // Step1：创建一个 QueryWrapper 对象
       QueryWrapper<User> queryWrapper = new QueryWrapper<>();
   
       // Step2： 构造查询条件
       queryWrapper
               .select("id", "name", "age")
               .eq("age", 20)
               .like("name", "j");
   
       // Step3：执行查询
       userService
               .list(queryWrapper)
               .forEach(System.out::println);
   }
   ```

   当前数据库数据如下：

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727182922.png)

   执行测试方法输出如下：

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200727182954.png)

   