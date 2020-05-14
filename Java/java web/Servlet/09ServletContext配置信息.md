# 为什么使用ServletContext
`ServletContext`的对象由Web容器在部署项目时创建。 该对象可用于从`web.xml`文件获取配置信息。 每个Web应用程序只有一个`ServletContext`对象。
如果有信息要共享给多个servlet使用，最好在`web.xml`文件中使用`<context-param>`元素提供它。

# ServletContext的优点
如果有任何信息要共享给所有的servlet使用，并且要让它容易维护，最好的办法就是在`web.xml`文件中提供这些信息，所以如果信息要更改直接在`web.xml`中修改，而不需要修改servlet代码。

**`ServletContext`接口的使用**

有很多ServletContext对象可以使用。 其中一些如下：
* ServletContext对象提供容器和servlet之间的接口。
* 使用ServletContext对象在`web.xml`文件获取配置信息。
* ServletContext对象可用于设置，获取或删除`web.xml`文件中属性。
* ServletContext对象可用于提供应用程序间通信。
![](../../../images/SevletContext的优点.jpg)

# 常用的`ServletContext`接口方法
给出了一些常用的`ServletContext`接口方法。
序号|方法|描述
--|--|--
1|`public String getInitParameter(String name)`|返回指定参数名称的参数值。
2|`public Enumeration getInitParameterNames()`|返回上下文的初始化参数的名称。
3|`public void setAttribute(String name,Object object)`|在应用程序范围内设置给定的对象。
4|`public Object getAttribute(String name)`|返回指定名称的属性。
5|`public Enumeration getInitParameterNames()`|返回上下文的初始化参数的名称，作为String对象的枚举。
6|`public void removeAttribute(String name)`|从servlet上下文中删除给定名称的属性。

# 如何获取`ServletContext`接口的对象?
 * 通过`ServletConfig`接口的`getServletContext()`方法返回`ServletContext`对象。
 * 通过`GenericServlet`类的`getServletContext()`方法返回`ServletContext`对象。

# `getServletContext()`方法的语法
~~~java
public ServletContext getServletContext()
~~~
# `getServletContext()`方法的示例
~~~java
ServletContext application=getServletConfig().getServletContext();  
 
ServletContext application=getServletContext();
~~~
# 在`Context`范围内提供初始化参数的语法
Web应用程序的`context-param`元素的子元素用于定义应用程序范围中的初始化参数。 参数名称和参数值是`context-param`的子元素。`param-name`元素定义参数名称，`param-value`定义其值。参考以下配置代码片段 -
~~~xml
<web-app>  
 ......  
  <context-param>  
    <param-name>parameter_name</param-name>  
    <param-value>parameter_value</param-value>  
  </context-param>
 ......  
</web-app>
~~~

# 获取初始化参数的`ServletContext`示例

ContextServlet.java
~~~java
public class ContextServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        // 设置服务器内容类型
        response.setContentType("text/html");
        // 获取服务器的输出对象
        PrintWriter out = response.getWriter();

        // 获取ServletContext对象
        ServletContext context = getServletContext();

        String driverName = context.getInitParameter("dname");

        out.println("driver name is <b>"+driverName+"</b>");
        out.close();
    }
}
~~~

Context.html
~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>ContextServlet</title>
</head>
<body>
<div style="text-align: center">
    请<a href="context">点击这里</a>查看Context信息
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
    <display-name>ContextServlet</display-name>
    <welcome-file-list>
        <welcome-file>Context.html</welcome-file>
    </welcome-file-list>
    <servlet>
        <servlet-name>ContextServlet</servlet-name>
        <servlet-class>ContextServlet</servlet-class>
    </servlet>
    <context-param>
        <param-name>dname</param-name>
        <param-value>com.mysql.jdbc.Driver</param-value>
    </context-param>
    <servlet-mapping>
        <servlet-name>ContextServlet</servlet-name>
        <url-pattern>/context</url-pattern>
    </servlet-mapping>
</we-app>
~~~



