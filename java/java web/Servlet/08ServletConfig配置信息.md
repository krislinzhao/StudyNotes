# 为什么使用ServletConfig
`ServletConfig`对象是由每个servlet的Web容器创建的。这个对象可用于从`web.xml`文件获取配置信息。
如果从`web.xml`文件修改配置信息，就不需要更改servlet中的代码。因此，对于有特定配置内容需要不定时修改，这样会更容易管理Web应用程序。

# ServletConfig的优点
`ServletConfig`的核心优点是，如果可以修改web.xml文件中的信息，则不需要编辑servlet代码文件。

**ServletConfig接口的方法**
编号|方法|描述
--|--|--
1|`public String getInitParameter(String name)`|返回指定参数名称的参数值。
2|`public Enumeration getInitParameterNames()`|返回所有初始化参数名称的枚举。
3|`public String getServletName()`|返回servlet的名称。
4|`public ServletContext getServletContext()`|返回ServletContext的对象。

# 如何获取ServletConfig的对象？

 * 可通过调用Servlet接口的`getServletConfig()`方法来返回`ServletConfig`对象。

# getServletConfig()方法的语法
~~~java
public ServletConfig getServletConfig();
~~~

# getServletConfig()方法的示例
~~~java
ServletConfig config=getServletConfig();  
~~~


# 为servlet提供初始化参数的语法
servlet的`init-param`子元素用于指定servlet的初始化参数。
~~~xml
<web-app>
  <servlet>
    ......
    <init-param>  
      <param-name>parameter_name</param-name>
      <param-value>parameter_value</param-value>
    </init-param>
    ......
  </servlet>
</web-app>
~~~

# 获取初始化参数的ServletConfig示例

ConfigServlet.java
~~~java
public class ConfigServlet extends HttpServlet {
    private static final long serialVersionUID =1L;

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        // 设置服务器内容类型
        response.setContentType("text/html");
        // 获取服务器输出对象
        PrintWriter out = response.getWriter();

        // 获取ServletConfig
        ServletConfig config = getServletConfig();

        String driver = config.getInitParameter("mysql_driver");
        out.println("driver details is <b>"+driver+"</b>");
        out.close();
    }
}
~~~

dbConfig.html
~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>ServletConfig</title>
</head>
<body>
    <div style="text-align: center">
        请<a href="dbConfig">点击这里</a>查看所有配置信息
    </div>
</body>
</html>
~~~

web.xml
~~~xml
<?xml version="1.0" encoding="UTF-8"?>

<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         id="WebApp_ID" version="3.1">
    <display-name>ServletRequest</display-name>
    <welcome-file-list>
        <welcome-file>dbConfig.html</welcome-file>
        <welcome-file>redirect.html</welcome-file>
        <welcome-file>forward.html</welcome-file>
        <welcome-file>head.html</welcome-file>
    </welcome-file-list>
    <servlet>
        <servlet-name>ConfigServlet</servlet-name>
        <servlet-class>ConfigServlet</servlet-class>
        <init-param>
            <param-name>mysql_driver</param-name>
            <param-value>com.mysql.jdbc.Driver</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>ConfigServlet</servlet-name>
        <url-pattern>/dbConfig</url-pattern>
    </servlet-mapping>
</web-app>
~~~





