# Spring学习笔记（一）入门案例

---
- [Spring学习笔记（一）入门案例](#spring%e5%ad%a6%e4%b9%a0%e7%ac%94%e8%ae%b0%e4%b8%80%e5%85%a5%e9%97%a8%e6%a1%88%e4%be%8b)
  - [1、实例](#1%e5%ae%9e%e4%be%8b)
    - [1. 通过 Idea 创建 maven 项目](#1-%e9%80%9a%e8%bf%87-idea-%e5%88%9b%e5%bb%ba-maven-%e9%a1%b9%e7%9b%ae)
    - [2. 配置 Spring 配置文件 applicationContext.xml](#2-%e9%85%8d%e7%bd%ae-spring-%e9%85%8d%e7%bd%ae%e6%96%87%e4%bb%b6-applicationcontextxml)
    - [3. 编写接口及实现类](#3-%e7%bc%96%e5%86%99%e6%8e%a5%e5%8f%a3%e5%8f%8a%e5%ae%9e%e7%8e%b0%e7%b1%bb)
    - [4. 编写测试类 Client](#4-%e7%bc%96%e5%86%99%e6%b5%8b%e8%af%95%e7%b1%bb-client)
  - [2、知识点](#2%e7%9f%a5%e8%af%86%e7%82%b9)
    - [1. ApplicationContext的三个常用实现类](#1-applicationcontext%e7%9a%84%e4%b8%89%e4%b8%aa%e5%b8%b8%e7%94%a8%e5%ae%9e%e7%8e%b0%e7%b1%bb)
      - [ClassPathXmlApplicationContext](#classpathxmlapplicationcontext)
      - [FileSyetemXmlApplicationContext](#filesyetemxmlapplicationcontext)
      - [AnnotationConfigApplicationContext](#annotationconfigapplicationcontext)
    - [2. 核心容器的两个接口引发出来的问题](#2-%e6%a0%b8%e5%bf%83%e5%ae%b9%e5%99%a8%e7%9a%84%e4%b8%a4%e4%b8%aa%e6%8e%a5%e5%8f%a3%e5%bc%95%e5%8f%91%e5%87%ba%e6%9d%a5%e7%9a%84%e9%97%ae%e9%a2%98)

---

## 1、实例

![spring入门案例项目结构](https://krislin.oss-cn-beijing.aliyuncs.com/pictures/study-notes-images/spring%E5%85%A5%E9%97%A8%E7%A8%8B%E5%BA%8F%E7%BB%93%E6%9E%84.jpg)

### 1. 通过 Idea 创建 maven 项目
### 2. 配置 Spring 配置文件 applicationContext.xml
### 3. 编写接口及实现类

- IaccountDao
~~~java
        /**
         * 账户的持久层接口
         */
        public interface IAccountDao {
        
            /**
             * 模拟保存账户
             */
            void saveAccount();
        }
~~~

- IaccountService
~~~java
        /**
         * 账户业务层的接口
         */
        public interface IAccountService {
        
            /**
             * 模拟保存账户
             */
            void saveAccount();
        }
~~~

- AccountDaoImpl
~~~java
        /**
         * 账户的持久层实现类
         */
        public class AccountDaoImpl implements IAccountDao {
        
            public  void saveAccount(){
        
                System.out.println("保存了账户");
            }
        }
~~~

- AccountServiceImpl
~~~java
        /**
        * 账户的业务层实现类
         */
        public class AccountServiceImpl implements IAccountService {
        
            private IAccountDao accountDao = new AccountDaoImpl();
        
            public void  saveAccount(){
                accountDao.saveAccount();
            }
        }
~~~

### 4. 编写测试类 Client
~~~java
        /**
        * 模拟一个表现层，用于调用业务层
        */
        public class Client {
            /**
            *
            *获取IOC的核心容器，并根据id获取对象
            * @param args
            */
            public static void main(String[] args) {
                ApplicationContext ac = new ClassPathXmlApplicationContext("beans.xml");
                // 两种不同的方式获取Bean对象
                IAccountService as = (IAccountService) ac.getBean("accountService");
                IAccountDao adao = ac.getBean("accountDao",IAccountDao.class);
                System.out.println(as);
                System.out.println(adao);
                        //        as.saveAccount();
            }
        }
~~~

## 2、知识点

### 1. ApplicationContext的三个常用实现类

####  ClassPathXmlApplicationContext
 它可以加载路径下的配置文件，要求配置文件必须在路径下，否则加载不了(**配置文件所在的目录必须要在IDEA中设置为任何类型的目录但是一般设置为Resource类型的目录**)

~~~java
ApplicationContext ac = new ClassPathXmlApplicationContext("beans.xml");
~~~

#### FileSyetemXmlApplicationContext
它可以加载磁盘下任意路径下的配置文件（必须有访问权限）

加载方式如下：
~~~java
ApplicationContext ac = new FileSystemXmlApplicationContex("C:\\user\\greyson\\...")
~~~

#### AnnotationConfigApplicationContext
它是用于读取注解创建容器的

### 2. 核心容器的两个接口引发出来的问题

- ApplicationContext：它在创建核心容器时，创建对象采取的策略是采用立即加载的方式，也就是说，只要一读取完配置文件就马上创建配置文件中配置的对象
    - 单例对象适用
    - 开发中常采用此接口
- BeanFactory: 它在构建核心容器时，创建对象的策略是采用延迟加载的方式，什么时候获取 id 对象了，什么时候就创建对象。
    - 多例对象适用
