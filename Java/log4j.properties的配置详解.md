# 一.参数意义说明
## 输出级别的种类
```properties
ERROR、WARN、INFO、DEBUG
ERROR 为严重错误 主要是程序的错误
WARN 为一般警告，比如session丢失
INFO 为一般要显示的信息，比如登录登出
DEBUG 为程序的调试信息
```
## 配置日志信息输出目的地
```properties
log4j.appender.appenderName = fully.qualified.name.of.appender.class
1.org.apache.log4j.ConsoleAppender（控制台）
2.org.apache.log4j.FileAppender（文件）
3.org.apache.log4j.DailyRollingFileAppender（每天产生一个日志文件）
4.org.apache.log4j.RollingFileAppender（文件大小到达指定尺寸的时候产生一个新的文件）
5.org.apache.log4j.WriterAppender（将日志信息以流格式发送到任意指定的地方）
```
## 配置日志信息的格式
```properties
log4j.appender.appenderName.layout = fully.qualified.name.of.layout.class
1.org.apache.log4j.HTMLLayout（以HTML表格形式布局），
2.org.apache.log4j.PatternLayout（可以灵活地指定布局模式），
3.org.apache.log4j.SimpleLayout（包含日志信息的级别和信息字符串），
4.org.apache.log4j.TTCCLayout（包含日志产生的时间、线程、类别等等信息）
```
## 控制台选项
```properties
Threshold=DEBUG:指定日志消息的输出最低层次。
ImmediateFlush=true:默认值是true,意谓着所有的消息都会被立即输出。
Target=System.err：默认情况下是：System.out,指定输出控制台
FileAppender 选项
Threshold=DEBUF:指定日志消息的输出最低层次。
ImmediateFlush=true:默认值是true,意谓着所有的消息都会被立即输出。
File=mylog.txt:指定消息输出到mylog.txt文件。
Append=false:默认值是true,即将消息增加到指定文件中，false指将消息覆盖指定的文件内容。
RollingFileAppender 选项
Threshold=DEBUG:指定日志消息的输出最低层次。
ImmediateFlush=true:默认值是true,意谓着所有的消息都会被立即输出。
File=mylog.txt:指定消息输出到mylog.txt文件。
Append=false:默认值是true,即将消息增加到指定文件中，false指将消息覆盖指定的文件内容。
MaxFileSize=100KB: 后缀可以是KB, MB 或者是 GB. 在日志文件到达该大小时，将会自动滚动，即将原来的内容移到mylog.log.1文件。
MaxBackupIndex=2:指定可以产生的滚动文件的最大数。
log4j.appender.A1.layout.ConversionPattern=%-4r %-5p %d{yyyy-MM-dd HH:mm:ssS} %c %m%n
```
## 日志信息格式中几个符号所代表的含义：
```properties
 -X号: X信息输出时左对齐；
 %p: 输出日志信息优先级，即DEBUG，INFO，WARN，ERROR，FATAL,
 %d: 输出日志时间点的日期或时间，默认格式为ISO8601，也可以在其后指定格式，比如：%d{yyy MMM dd HH:mm:ss,SSS}，输出类似：2002年10月18日 22：10：28，921
 %r: 输出自应用启动到输出该log信息耗费的毫秒数
 %c: 输出日志信息所属的类目，通常就是所在类的全名
 %t: 输出产生该日志事件的线程名
 %l: 输出日志事件的发生位置，相当于%C.%M(%F:%L)的组合,包括类目名、发生的线程，以及在代码中的行数。举例：Testlog4.main (TestLog4.java:10)
 %x: 输出和当前线程相关联的NDC(嵌套诊断环境),尤其用到像java servlets这样的多客户多线程的应用中。
 %%: 输出一个"%"字符
 %F: 输出日志消息产生时所在的文件名称
 %L: 输出代码中的行号
 %m: 输出代码中指定的消息,产生的日志具体信息
 %n: 输出一个回车换行符，Windows平台为"/r/n"，Unix平台为"/n"输出日志信息换行
 可以在%与模式字符之间加上修饰符来控制其最小宽度、最大宽度、和文本的对齐方式。如：
 1)%20c：指定输出category的名称，最小的宽度是20，如果category的名称小于20的话，默认的情况下右对齐。
 2)%-20c:指定输出category的名称，最小的宽度是20，如果category的名称小于20的话，"-"号指定左对齐。
 3)%.30c:指定输出category的名称，最大的宽度是30，如果category的名称大于30的话，就会将左边多出的字符截掉，但小于30的话也不会有空格。
 4)%20.30c:如果category的名称小于20就补空格，并且右对齐，如果其名称长于30字符，就从左边较远输出的字符截掉。
```
# 二.文件配置
## Sample1
```properties
log4j.rootLogger=DEBUG,A1,R
#log4j.rootLogger=INFO,A1,R
# ConsoleAppender 输出
log4j.appender.A1=org.apache.log4j.ConsoleAppender
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%-d{yyyy-MM-dd HH:mm:ss,SSS} [%c]-[%p] %m%n
# File 输出 一天一个文件,输出路径可以定制,一般在根路径下
log4j.appender.R=org.apache.log4j.DailyRollingFileAppender
log4j.appender.R.File=blog_log.txt
log4j.appender.R.MaxFileSize=500KB
log4j.appender.R.MaxBackupIndex=10
log4j.appender.R.layout=org.apache.log4j.PatternLayout
log4j.appender.R.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSS} [%t] [%c] [%p] - %m%n
```
## Sample2
下面给出的Log4J配置文件实现了输出到控制台，文件，回滚文件，发送日志邮件，输出到数据库日志表，自定义标签等全套功能。
~~~properties
    log4j.rootLogger=DEBUG,CONSOLE,A1,im
    #DEBUG,CONSOLE,FILE,ROLLING_FILE,MAIL,DATABASE
    log4j.addivity.org.apache=true
    ###################
    # Console Appender
    ###################
    log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
    log4j.appender.Threshold=DEBUG
    log4j.appender.CONSOLE.Target=System.out
    log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
    log4j.appender.CONSOLE.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n
    #log4j.appender.CONSOLE.layout.ConversionPattern=[start]%d{DATE}[DATE]%n%p[PRIORITY]%n%x[NDC]%n%t[THREAD] n%c[CATEGORY]%n%m[MESSAGE]%n%n
    #####################
    # File Appender
    #####################
    log4j.appender.FILE=org.apache.log4j.FileAppender
    log4j.appender.FILE.File=file.log
    log4j.appender.FILE.Append=false
    log4j.appender.FILE.layout=org.apache.log4j.PatternLayout
    log4j.appender.FILE.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n
    # Use this layout for LogFactor 5 analysis
    ########################
    # Rolling File
    ########################
    log4j.appender.ROLLING_FILE=org.apache.log4j.RollingFileAppender
    log4j.appender.ROLLING_FILE.Threshold=ERROR
    log4j.appender.ROLLING_FILE.File=rolling.log
    log4j.appender.ROLLING_FILE.Append=true
    log4j.appender.ROLLING_FILE.MaxFileSize=10KB
    log4j.appender.ROLLING_FILE.MaxBackupIndex=1
    log4j.appender.ROLLING_FILE.layout=org.apache.log4j.PatternLayout
    log4j.appender.ROLLING_FILE.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n
    ####################
    # Socket Appender
    ####################
    log4j.appender.SOCKET=org.apache.log4j.RollingFileAppender
    log4j.appender.SOCKET.RemoteHost=localhost
    log4j.appender.SOCKET.Port=5001
    log4j.appender.SOCKET.LocationInfo=true
    # Set up for Log Facter 5
    log4j.appender.SOCKET.layout=org.apache.log4j.PatternLayout
    log4j.appender.SOCET.layout.ConversionPattern=[start]%d{DATE}[DATE]%n%p[PRIORITY]%n%x[NDC]%n%t[THREAD]%n%c[CATEGORY]%n%m[MESSAGE]%n%n
    ########################
    # Log Factor 5 Appender
    ########################
    log4j.appender.LF5_APPENDER=org.apache.log4j.lf5.LF5Appender
    log4j.appender.LF5_APPENDER.MaxNumberOfRecords=2000
    ########################
    # SMTP Appender
    #######################
    log4j.appender.MAIL=org.apache.log4j.net.SMTPAppender
    log4j.appender.MAIL.Threshold=FATAL
    log4j.appender.MAIL.BufferSize=10
    log4j.appender.MAIL.From=chenyl@yeqiangwei.com
    log4j.appender.MAIL.SMTPHost=mail.hollycrm.com
    log4j.appender.MAIL.Subject=Log4J Message
    log4j.appender.MAIL.To=chenyl@yeqiangwei.com
    log4j.appender.MAIL.layout=org.apache.log4j.PatternLayout
    log4j.appender.MAIL.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n
    ########################
    # JDBC Appender
    #######################
    log4j.appender.DATABASE=org.apache.log4j.jdbc.JDBCAppender
    log4j.appender.DATABASE.URL=jdbc:mysql://localhost:3306/test
    log4j.appender.DATABASE.driver=com.mysql.jdbc.Driver
    log4j.appender.DATABASE.user=root
    log4j.appender.DATABASE.password=
    log4j.appender.DATABASE.sql=INSERT INTO LOG4J (Message) VALUES ('[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n')
    log4j.appender.DATABASE.layout=org.apache.log4j.PatternLayout
    log4j.appender.DATABASE.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n
    log4j.appender.A1=org.apache.log4j.DailyRollingFileAppender
    log4j.appender.A1.File=SampleMessages.log4j
    log4j.appender.A1.DatePattern=yyyyMMdd-HH'.log4j'
    log4j.appender.A1.layout=org.apache.log4j.xml.XMLLayout
    ###################
    #自定义Appender
    ###################
    log4j.appender.im = net.cybercorlin.util.logger.appender.IMAppender
    log4j.appender.im.host = mail.cybercorlin.net
    log4j.appender.im.username = username
    log4j.appender.im.password = password
    log4j.appender.im.recipient = corlin@yeqiangwei.com
    log4j.appender.im.layout=org.apache.log4j.PatternLayout
    log4j.appender.im.layout.ConversionPattern =[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n
~~~
# 三.高级使用：
 1. 把FATAL级错误写入2000NT日志
 2. WARN，ERROR，FATAL级错误发送email通知管理员
 3. 其他级别的错误直接在后台输出

 ## 输出到2000NT日志
 ### 1.把Log4j压缩包里的NTEventLogAppender.dll拷到WINNT/SYSTEM32目录下
 ### 2.写配置文件log4j.properties
```properties
# 在2000系统日志输出
log4j.logger.NTlog=FATAL, A8
#APPENDER A8
log4j.appender.A8=org.apache.log4j.nt.NTEventLogAppender
log4j.appender.A8.Source=JavaTest
log4j.appender.A8.layout=org.apache.log4j.PatternLayout
log4j.appender.A8.layout.ConversionPattern=%-4r %-5p [%t] %37c %3x - %m%n
```
 ### 3.调用代码：
```java
 Logger logger2 = Logger.getLogger("NTlog"); //要和配置文件中设置的名字相同
 logger2.debug("debug!!!");
 logger2.info("info!!!");
 logger2.warn("warn!!!");
 logger2.error("error!!!");
 //只有这个错误才会写入2000日志
 logger2.fatal("fatal!!!");
```
## 发送email通知管理员：
 ### 1.首先下载JavaMail和JAF

  http://java.sun.com/j2ee/ja/javamail/index.html
  http://java.sun.com/beans/glasgow/jaf.html

 在项目中引用mail.jar和activation.jar。

 ### 2.写配置文件
```properties
# 将日志发送到email
log4j.logger.MailLog=WARN,A5
# APPENDER A5
log4j.appender.A5=org.apache.log4j.net.SMTPAppender
log4j.appender.A5.BufferSize=5
log4j.appender.A5.To=chunjie@yeqiangwei.com
log4j.appender.A5.From=error@yeqiangwei.com
log4j.appender.A5.Subject=ErrorLog
log4j.appender.A5.SMTPHost=smtp.263.net
log4j.appender.A5.layout=org.apache.log4j.PatternLayout
log4j.appender.A5.layout.ConversionPattern=%-4r %-5p [%t] %37c %3x - %m%n
```
 ### 3.调用代码：
```java
 //把日志发送到mail
 Logger logger3 = Logger.getLogger("MailLog");
 logger3.warn("warn!!!");
 logger3.error("error!!!");
 logger3.fatal("fatal!!!");
```
## 在后台输出所有类别的错误
 ### 1. 写配置文件
```properties
# 在后台输出
log4j.logger.console=DEBUG, A1
#APPENDER A1
log4j.appender.A1=org.apache.log4j.ConsoleAppender
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%-4r %-5p [%t] %37c %3x - %m%n
```
 ### 2．调用代码
```java
 Logger logger1 = Logger.getLogger("console");
 logger1.debug("debug!!!");
 logger1.info("info!!!");
 logger1.warn("warn!!!");
 logger1.error("error!!!");
 logger1.fatal("fatal!!!");
```
## 全部配置文件：log4j.properties
```properties
 #在后台输出
 log4j.logger.console=DEBUG, A1
 # APPENDER A1
 log4j.appender.A1=org.apache.log4j.ConsoleAppender
 log4j.appender.A1.layout=org.apache.log4j.PatternLayout
 log4j.appender.A1.layout.ConversionPattern=%-4r %-5p [%t] %37c %3x - %m%n
# 在2000系统日志输出
 log4j.logger.NTlog=FATAL, A8
 # APPENDER A8
 log4j.appender.A8=org.apache.log4j.nt.NTEventLogAppender
 log4j.appender.A8.Source=JavaTest
 log4j.appender.A8.layout=org.apache.log4j.PatternLayout
 log4j.appender.A8.layout.ConversionPattern=%-4r %-5p [%t] %37c %3x - %m%n
# 将日志发送到email
 log4j.logger.MailLog=WARN,A5
 #  APPENDER A5
 log4j.appender.A5=org.apache.log4j.net.SMTPAppender
 log4j.appender.A5.BufferSize=5
 log4j.appender.A5.To=chunjie@yeqiangwei.com
 log4j.appender.A5.From=error@yeqiangwei.com
 log4j.appender.A5.Subject=ErrorLog
 log4j.appender.A5.SMTPHost=smtp.263.net
 log4j.appender.A5.layout=org.apache.log4j.PatternLayout
 log4j.appender.A5.layout.ConversionPattern=%-4r %-5p [%t] %37c %3x - %m%n
```
## 全部代码：Log4jTest.java

```java
import org.apache.log4j.*;
//import org.apache.log4j.nt.*;
//import org.apache.log4j.net.*;

public class Log4jTest
{
    public static void main(String args[])
    {
        PropertyConfigurator.configure("log4j.properties");
        //在后台输出
        Logger logger1 = Logger.getLogger("console");
        logger1.debug("debug!!!");
        logger1.info("info!!!");
        logger1.warn("warn!!!");
        logger1.error("error!!!");
        logger1.fatal("fatal!!!");
        //在NT系统日志输出
        Logger logger2 = Logger.getLogger("NTlog");
        //NTEventLogAppender nla = new NTEventLogAppender();
        logger2.debug("debug!!!");
        logger2.info("info!!!");
        logger2.warn("warn!!!");
        logger2.error("error!!!");
        //只有这个错误才会写入2000日志
        logger2.fatal("fatal!!!");
        //把日志发送到mail
        Logger logger3 = Logger.getLogger("MailLog");
        //SMTPAppender sa = new SMTPAppender();
        logger3.warn("warn!!!");
        logger3.error("error!!!");
        logger3.fatal("fatal!!!");
    }
}
```