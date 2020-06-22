# Statement和PreparedStatement的区别

和 Statement一样，PreparedStatement也是用来执行sql语句的
与创建Statement不同的是，需要根据sql语句创建PreparedStatement
除此之外，还能够通过设置参数，指定相应的值，而不是Statement那样使用字符串拼接

注： 这是JAVA里唯二的基1的地方，另一个是查询语句中的ResultSet也是基1的。

##PreparedStatement的优点
1.参数设置

Statement 需要进行字符串拼接，可读性和维护性比较差
~~~
String sql = "insert into hero values(null,"+"'提莫'"+","+313.0f+","+50+")";
~~~

PreparedStatement 使用参数设置，可读性好，不易犯错
~~~
String sql = "insert into hero values(null,?,?,?)";
~~~

2.性能表现

 PreparedStatement有预编译机制，性能比Statement更快

1. 防止SQL注入式攻击

假设name是用户提交来的数据
~~~
String name = "'盖伦' OR 1=1";
~~~

使用Statement就需要进行字符串拼接
拼接出来的语句是：
~~~ 
select * from hero where name = '盖伦' OR 1=1
~~~
因为有OR 1=1，这是恒成立的
那么就会把所有的英雄都查出来，而不只是盖伦
如果Hero表里的数据是海量的，比如几百万条，把这个表里的数据全部查出来
会让数据库负载变高，CPU100%，内存消耗光，响应变得极其缓慢

而PreparedStatement使用的是参数设置，就不会有这个问题

# execute与executeUpdate的区别
## 相同点
execute与executeUpdate的相同点：都可以执行增加，删除，修改
##不同点
* 不同1：

    execute可以执行查询语句
    然后通过getResultSet，把结果集取出来
    executeUpdate不能执行查询语句
* 不同2:

    execute返回boolean类型，true表示执行的是查询语句，false表示执行的是insert,delete,update等等
    executeUpdate返回的是int，表示有多少条数据受到了影响

# 特殊操作
## 获取自增长id
在Statement通过execute或者executeUpdate执行完插入语句后，MySQL会为新插入的数据分配一个自增长id，(前提是这个表的id设置为了自增长,在Mysql创建表的时候，AUTO_INCREMENT就表示自增长)
~~~
CREATE TABLE hero (
  id int(11) AUTO_INCREMENT,
  ...
}
~~~


但是无论是execute还是executeUpdate都不会返回这个自增长id是多少。需要通过Statement的getGeneratedKeys获取该id
注： 后面加了个Statement.RETURN_GENERATED_KEYS参数，以确保会返回自增长ID。 通常情况下不需要加这个，有的时候需要加，所以先加上，保险一些
~~~
PreparedStatement ps = c.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);
~~~
## 获取表的元数据
元数据概念：

和数据库服务器相关的数据，比如数据库版本，有哪些表，表有哪些字段，字段类型是什么等等。
~~~
// 查看数据库层面的元数据
// 即数据库服务器版本，驱动版本，都有哪些数据库等等

DatabaseMetaData dbmd = c.getMetaData();

// 获取数据库服务器产品名称
dbmd.getDatabaseProductName();
// 获取数据库服务器产品版本号
dbmd.getDatabaseProductVersion();
// 获取数据库服务器用作类别和表名之间的分隔符 如test.user
dbmd.getCatalogSeparator();
// 获取驱动版本
dbmd.getDriverVersion();

System.out.println("可用的数据库列表：");
// 获取数据库名称
ResultSet rs = dbmd.getCatalogs();
while (rs.next()) {
    System.out.println("数据库名称:\t"+rs.getString(1));
}

~~~

# ORM
ORM=Object Relationship Database Mapping

对象和关系数据库的映射

简单说，一个对象，对应数据库里的一条记录

# DAO
DAO=Data Access Object

数据访问对象

实际上就是运用了练习-ORM中的思路，把数据库相关的操作都封装在这个类里面，其他地方看不到JDBC的代码

# 数据库连接池
## 数据库连接池原理-使用池
连接池在使用之前，就会创建好一定数量的连接。
如果有任何线程需要使用连接，那么就从连接池里面借用，而不是自己重新创建.
使用完毕后，又把这个连接归还给连接池供下一次或者其他线程使用。
倘若发生多线程并发情况，连接池里的连接被借用光了，那么其他线程就会临时等待，直到有连接被归还回来，再继续使用。
整个过程，这些连接都不会被关闭，而是不断的被循环使用，从而节约了启动和关闭连接的时间。
## ConnectionPool构造方法和初始化
1. ConnectionPool() 构造方法约定了这个连接池一共有多少连接

2. 在init() 初始化方法中，创建了size条连接。 注意，这里不能使用try-with-resource这种自动关闭连接的方式，因为连接恰恰需要保持不关闭状态，供后续循环使用

3. getConnection， 判断是否为空，如果是空的就wait等待，否则就借用一条连接出去

4. returnConnection， 在使用完毕后，归还这个连接到连接池，并且在归还完毕后，调用notifyAll，通知那些等待的线程，有新的连接可以借用了。
