# 一、配置文件规划

我们开发的服务通常会部署在不同的环境中，例如开发环境、测试环境，生产环境等，而不同环境需要不同的配置，例如连接不同的 `Redis`、数据库、第三方服务等等。Spring Boot 默认的配置文件是 `application.properties`(或`yml`)。那么如何实现不同的环境使用不同的配置文件呢？一个比较好的实践是为不同的环境定义不同的配置文件，如下所示：
![](https://cdn.jsdelivr.net/gh/krislinzhao/IMGcloud/img/20200421134029.png)

全局配置文件:`application.yml`
开发环境配置文件：`application-dev.yml`
测试环境配置文件：`application-test.yml`
生产环境配置文件：`application-prod.yml`

# 二、yml支持多文档块方式

每个文档块使用`---`分割

```yaml
server:
  port: 8080
spring:
  profiles:
    active: prod
---
server:
  port: 8081
spring:
  profiles: dev
---
server:
  port: 8082
spring:
  profiles: prod
```

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200524094109.png)

# 三、切换环境的方式

## 1. 通过配置`application.yml`

`application.yml`是默认使用的配置文件，在其中通过`spring.profiles.active`设置使用哪一个配置文件，下面代码表示使用`application-prod.yml`配置，如果`application-prod.yml`和`application.yml`配置了相同的配置，比如都配置了运行端口，那`application-prod.yml`的优先级更高

```yaml
#需要使用的配置文件
spring:
  profiles:
    active: prod
```

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200524094155.png)

## 2. VM options、Program arguments、Active Profile

VM options设置启动参数 `-Dspring.profiles.active=prod`

Program arguments设置 `--spring.profiles.active=prod`

Active Profile 设置 prod

**这三个参数不要一起设置，会引起冲突，选一种即可**，如下图

![](https://cdn.jsdelivr.net/gh/krislinzhao/IMGcloud/img/20200421134611.png)

## 3.命令行方式

将项目打成jar包，在jar包的目录下打开命令行，使用如下命令启动：

```
java -jar spring-boot-profile.jar --spring.profiles.active=prod
```

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200524101157.png)

关于 Spring Profiles 更多信息可以参见：[Spring Profiles](https://www.baeldung.com/spring-profiles)