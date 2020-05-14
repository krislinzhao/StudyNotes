# 一、为什么要做bean赋值转换

![](https://cdn.jsdelivr.net/gh/krislinzhao/IMGcloud/img/20200422161747.png)
**PO:** persistent object 持久对象，对应数据库中的entity。通常在进行数据库数据存取操作时使用。可以简单的认为一个PO对应数据库中一张表中的一个记录。PO对象里面只有基本数据类型和String类型的属性（如：int、String），与数据库字段是一一对应的。
**BO:** business object 业务对象，业务对象主要作用是把业务逻辑封装为一个对象。这个对象可以包括一个或多个其它的对象。通常一个BO是多个PO的组合体，比如：PO在查询出来之后，需要经过业务处理，处理过程中对象的属性逐渐变复杂，有嵌套的数组，对象数组等等。
**VO:** view object，主要与web页面的展示结构相对应，所以VO也是前端与后端的数据交换定义。

下图中是一个VO，用于返回给前端web界面，用于渲染的数据内容：
![](https://cdn.jsdelivr.net/gh/krislinzhao/IMGcloud/img/20200422162011.png)
下图是一个PO，用于对数据库表的数据的存取。
![](https://cdn.jsdelivr.net/gh/krislinzhao/IMGcloud/img/20200422162053.png)

大家注意看二者的区别，一个AricleVO不仅包含了Article的数据，还包含了Reader读者的数据。也就是说，当你需要向数据库里面插入数据的时候，你需要将Article(PO)和Reader(PO)分别作为PO记录插入数据库。当你需要将一篇文章的数据和读者信息返回给页面做渲染的时候，你需要从数据库里面查询Article(PO)和Reader(PO),然后将二者组合映射转换为AricleVO返回给前端。

**如果你的业务，可以用一个实体类对象，就可以贯穿持久层到展现层，就没有必要做映射赋值转换，也没有必要去分VO、BO、PO。比如：单表表格数据展现、修改、新增。**

## 二、Dozer是什么？

- dozer是一个能把实体和实体之间进行转换的工具.只要建立好映射关系.就像是ORM的数据库和实体映射一样.

使用方法示例如下:

```java
    Mapper mapper = DozerBeanMapperBuilder.buildDefault();
    // article(PO) -> articleVO
    ArticleVO articleVO = mapper.map(article, ArticleVO.class);
```

这段示例代码，将从数据库里面查询得到的PO对象article，转换为VO对象articleVO，转换过程将所有同名同类型的数据自动赋值给articleVO的成员变量，当然除了reader(因为PO里面没有reader数组数据)。转换需要写属性之间的映射么?不! 默认是根据属性名称来匹配的.
如果没有Dozer我们进行，对象之间的转换赋值，我们会怎么做？下面的这5行等于上面的一行。

```java
articleVO.setId(article.getId());
articleVO.setAuthor(article.getAuthor());
articleVO.setTitle(article.getTitle());
articleVO.setContent(article.getContent());
articleVO.setCreateTime(article.getCreateTime());
```

## 三、引入Dozer（6.2.0）

从6.2.0版本开始，dozer官方为我们提供了dozer-spring-boot-starter，这样我们在spring boot里面使用dozer更方便了。

```xml
<dependency>
    <groupId>com.github.dozermapper</groupId>
    <artifactId>dozer-spring-boot-starter</artifactId>
    <version>6.2.0</version>
</dependency>
```

在实际开发中，我们不只需要PO转VO，有时还需要`List`转`List`.写一个工具类，实现List转List

```java
public class DozerUtils {

    static Mapper mapper = DozerBeanMapperBuilder.buildDefault();

    public static <T> List<T> mapList(Collection sourceList, Class<T> destinationClass){
        List destinationList = Lists.newArrayList();
        for (Iterator i$ = sourceList.iterator(); i$.hasNext();){
            Object sourceObject = i$.next();
            Object destinationObject = mapper.map(sourceObject, destinationClass);
            destinationList.add(destinationObject);
        }
        return destinationList;
    }
}
```

## 四、自定义类型转换（非对称类型转换）

在平时的开发中，我们的VO和PO的同名字段尽量是类型一致的。String属性->String属性，Date属性 -> Date属性，但是也不排除有的朋友由于最开始的设计失误

- 需要String属性 -> Date属性，或者ClassA转ClassB呢？这种我们该如何实现呢？
- 或者需要createDate 转 cDate这种属性名称都不一样的，怎么做。

比如下面的两个测试model，进行属性自动赋值转换映射。

```java
@Data
@AllArgsConstructor
public class TestA{
    public String name;
    public String createDate;  //注意这里名称不一样，类型不一样
}
@Data
@NoArgsConstructor
public class TestB{
    public String name;
    public Date cDate;    //注意这里名称不一样，类型不一样
}
```

然后，我们需要自己去创建转换对应关系，比如：resources/dozer/dozer-mapping.xml。xml内容看上去复杂，其实核心结构很简单。就是class-a到classb的转换，filed用来定义特殊字段（名称或类型不一致）。configuration可以做全局的配置，date-format对所有的日期字符串转换生效。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<mappings xmlns="http://dozermapper.github.io/schema/bean-mapping"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://dozermapper.github.io/schema/bean-mapping
          http://dozermapper.github.io/schema/bean-mapping.xsd">
    <configuration>
        <date-format>yyyy-MM-dd HH:mm:ss</date-format>
    </configuration>
    <mapping>
        <class-a>club.krislin.bootlaunch.dozer.TestA</class-a>
        <class-b>club.krislin.bootlaunch.dozer.TestB</class-b>
        <field>
            <a>createDate</a>
            <b>cDate</b>
        </field>
    </mapping>

</mappings>
```

然后把dozer转换配置文件通知application.yml，进行加载生效

```yaml
dozer:
  mapping-files: classpath:/dozer/dozer-mapping.xml
```

这样一个对象里面有String属性到Date属性转换的时候，就会自动应用这个转换规则， 不再报错。

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class DozerTests {

    @Test
    public void dozerTests() {
        Mapper mapper = DozerBeanMapperBuilder
                .create().withMappingFiles("dozer/dozer-mapping.xml")
                .build();
        TestA testA = new TestA("kobe","2020-03-08 11:25:25");
        
        System.out.println(mapper.map(testA,TestB.class));
    }
}
```

输出：

```
TestB(name=kobe, cDate=Sun Mar 08 11:25:25 CST 2020)
```