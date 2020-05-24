# 一、全局配置文件

SpringBoot使用一个全局的配置文件，配置文件名是固定的；

- application.properties
- application.yml

**全局配置文件的作用**：修改SpringBoot自动配置的默认值，SpringBoot在底层给我们做自动加载。

# 二、配置加载原理源码解析

所有的Spring Boot应用程序都是以`SpringApplication.run()`作为应用程序入口的。下面我们来一步一步跟踪一下这个函数。

```java
public static ConfigurableApplicationContext run(Class<?> primarySource, String... args) {
    return run(new Class[]{primarySource}, args);
}

public static ConfigurableApplicationContext run(Class<?>[] primarySources, String[] args) {
    return (new SpringApplication(primarySources)).run(args);
}
```

run方法传入了`SpringApplication`对象和一些运行期参数。继续向前跟进，我们发现一个类叫做`SpringFactoriesLoader`，这里面体现了Spring Boot加载配置文件的核心逻辑。
![](https://cdn.jsdelivr.net/gh/krislinzhao/IMGcloud/img//20200417144710.png)

从上图可以看到：

- 从`META-INF/spring.factories`文件夹下下面加载了spring.factories文件资源
- 然后读取文件中的ClassName作为值放入Properties。

![](https://cdn.jsdelivr.net/gh/krislinzhao/IMGcloud/img//20200417145104.png)
然后通过反射机制，对spring.factories里面的类资源进行实例化。

# 三、@EnableAutoConfiguration 作用

* 利用EnableAutoConfigurationImportSelector给容器中导入一些组件

* getAutoConfigurationEntry方法中

  ```java
  //获取候选的配置
  List<String> configurations = getCandidateConfigurations(annotationMetadata,attributes);
  ```

* SpringBoot入口启动类使用了`SpringBootApplication`，实际上就是开启了自动配置功能`@EnableAutoConfiguration`。

![](https://cdn.jsdelivr.net/gh/krislinzhao/IMGcloud/img/20200417150852.png)

`SpringFactoriesLoader`会以`@EnableAutoConfiguration`的包名和类名`org.springframework.boot.autoconfigure.EnableAutoConfiguration`为Key查找`spring.factories`文件,并将value中的类名实例化加载到Spring Boot应用中。如下图：
![](https://cdn.jsdelivr.net/gh/krislinzhao/IMGcloud/img/20200417152737.png)

* 每一个这样的 `xxxAutoConfiguration`类都是容器中的一个组件，都加入到容器中；用他们来做自动配置；
* 每一个自动配置类进行自动配置功能；

# 四、HttpEncodingAutoConfiguration自动配置原理

每一个自动配置类进行自动配置功能（`spring.factories`中的每一行对应的类）,我们以`HttpEncodingAutoConfiguration`为例讲解一下：

```java
package org.springframework.boot.autoconfigure.web.servlet;

......

//表示这是一个配置类，以前编写的配置文件一样，也可以给容器中添加组件
@Configuration(
    proxyBeanMethods = false
)

/**
 * 启动指定类的ConfigurationProperties功能；
 * 将配置文件中对应的值和HttpProperties绑定起来；
 * 并把HttpProperties加入到ioc容器中
 */
@EnableConfigurationProperties({HttpProperties.class})

/**
 * Spring底层@Conditional注解
 * 根据不同的条件，如果满足指定的条件，整个配置类里面的配置就会生效；
 * 判断当前应用是否是web应用，如果是，当前配置类生效
 */
@ConditionalOnWebApplication(
    type = Type.SERVLET
)

//判断当前项目有没有这个类
@ConditionalOnClass({CharacterEncodingFilter.class})

/**
 * 判断配置文件中是否存在某个配置  spring.http.encoding.enabled；如果不存在，判断也是成立的
 * 即使我们配置文件中不配置spring.http.encoding.enabled=true，也是默认生效的；
 */
@ConditionalOnProperty(
    prefix = "spring.http.encoding",
    value = {"enabled"},
    matchIfMissing = true
)
public class HttpEncodingAutoConfiguration {

    //它已经和SpringBoot的配置文件映射了
    private final Encoding properties;

    //只有一个有参构造器的情况下，参数的值就会从容器中拿
    public HttpEncodingAutoConfiguration(HttpProperties properties) {
        this.properties = properties.getEncoding();
    }

    @Bean     //给容器中添加一个组件，这个组件的某些值需要从properties中获取
    @ConditionalOnMissingBean    //判断容器有没有这个组件？（容器中没有才会添加这个组件）
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.RESPONSE));
        return filter;
    }

    ......
```

1. 根据当前不同的条件判断，决定这个配置类是否生效
2. 一但这个配置类生效；这个配置类就会给容器中添加各种组件；这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的；

**所有在配置文件中能配置的属性都是在`xxxxProperties`类中封装着；配置文件能配置什么就可以参照某个功能对应的这个属性类**

```java
@ConfigurationProperties(
    prefix = "spring.http"
)
public class HttpProperties {
    private boolean logRequestDetails;
    private final HttpProperties.Encoding encoding = new HttpProperties.Encoding();
```

# 五、总结

- SpringBoot启动会加载大量的自动配置类
- 我们看我们需要的功能有没有SpringBoot默认写好的自动配置类
- 再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件有，我们就不需要再来配置了）
- 给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们就可以在配置文件中指定这些属性的值

`xxxxAutoConfigurartion`：自动配置类；

`xxxxProperties`:封装配置文件中相关属性；

# @Conditional派生注解

作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；

| @Conditional扩展注解            | 作用（判断是否满足当前指定条件）                             |
| ------------------------------- | ------------------------------------------------------------ |
| @ConditionalOnJava              | 指定的Java版本存在时，才执行实例化方法或将类实例化           |
| @ConditionalOnBean              | DI容器中存在该类型Bean时，才执行实例化方法或将类实例化       |
| @ConditionalOnMissingBean       | DI容器中不存在该类型Bean时，才执行实例化方法或将类实例化     |
| @ConditionalOnExpression        | SpEL表达式结果为true时，才执行实例化方法或将类实例化         |
| @ConditionalOnClass             | classpath中存在该类时，才执行实例化方法或将类实例化          |
| @ConditionalOnMissingClass      | classpath中不存在该类时，才执行实例化方法或将类实例化        |
| @ConditionalOnSingleCandidate   | DI容器中该类型Bean只有一个或@Primary的只有一个时，才执行实例化方法或将类实例化 |
| @ConditionalOnProperty          | 参数设置或者值一致时，才执行实例化方法或将类实例化           |
| @ConditionalOnResource          | 指定的文件存在时，才执行实例化方法或将类实例化               |
| @ConditionalOnWebApplication    | Web应用环境下，才执行实例化方法或将类实例化                  |
| @ConditionalOnNotWebApplication | 非Web应用环境下，才执行实例化方法或将类实例化                |
| @ConditionalOnJndi              | 指定的JNDI存在时，才执行实例化方法或将类实例化               |

> 我们讲的这个实现原理实际上就是一个自定义spring-boot-starter的实现原理，我们会在后面章节中自己编码实现一个分布式文件系统fastdfs与spring boot整合的starter。大家届时会有更深一步的理解。在以上的自动装配过程中依赖于HttpEncodingProperties的自定义属性，我们后面会讲如何读取自定义配置属性。

# 查看那些自动配置类生效了

自动配置类必须在一定的条件下才能生效；

我们怎么知道哪些自动配置类生效了；

我们可以通过配置文件启用 `debug=true`属性；来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效；

- `Positive matches` ：（自动配置类启用的）
- `Negative matches`：（没有启动，没有匹配成功的自动配置类）