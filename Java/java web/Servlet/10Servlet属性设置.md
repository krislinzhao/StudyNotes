Servlet中的属性是可以从以下范围之一设置，获取或删除的对象：

* 请求范围
* 会话范围
* 应用范围
  
Servlet程序员可以使用属性将信息从一个servlet传递给另一个servlet。它就像将对象从一个类传递给另一个类一样，以便我们可以一次又一次地重用同一个对象。

# ServletRequest，HttpSession和ServletContext接口的属性特定方法
Servlet中有以下`4`种属性方法，它们具体如下：
序号|方法|描述
--|--|--
1|`public void setAttribute(String name,Object object)`|在应用程序范围内设置给定的对象。
2|`public Object getAttribute(String name)`|返回指定名称的属性。
3|`public Enumeration getInitParameterNames()`|返回上下文的初始化参数的名称，转为String对象的枚举。
4|`public void removeAttribute(String name)`|从servlet上下文中删除具有给定名称的属性。

# ServletConfig和ServletContext的区别
`servletconfig`对象引用单个`servlet`，而`servletcontext`对象引用整个Web应用程序。

# `ServletContext`的示例设置和获取属性

ServletAttr.java
~~~java
public class ServletAttr extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response){
        try {
            // 设置服务器端内容类型
            response.setContentType("text/html");
            // 设置服务器端编码方式
            response.setCharacterEncoding("utf-8");
            // 获取服务器端输出对象
            PrintWriter out = response.getWriter();

            // 获取ServletContext对象
            ServletContext context = getServletContext();
            // 设置属性
            context.setAttribute("school","WTU");

            out.println("Welcome to first servlet");
            out.println("在第二个Servlet<a href='servletAttr2'>查看属性值</a>");
            out.close();

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
~~~

ServletAttr2.java
~~~java
public class ServletAttr2 extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        // 设置服务器端内容类型和编码方式
        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");
        // 获取服务器端的输出对象
        PrintWriter out = response.getWriter();
        // 获取ServletContext对象
        ServletContext context = getServletContext();

        String name = (String) context.getAttribute("school");

        out.println("Welcome to "+name);
        out.close();
    }
}
~~~

web.xml
~~~xml
<?xml version="1.0" encoding="UTF-8"?>

<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         id="WebApp_ID" version="3.1">
    <display-name>AttributeServlet</display-name>
    <welcome-file-list>
    </welcome-file-list>
    <servlet>
        <servlet-name>AttributeServlet</servlet-name>
        <servlet-class>ServletAttr</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>AttributeServlet</servlet-name>
        <url-pattern>/servletAttr</url-pattern>
    </servlet-mapping>
    <servlet>
        <servlet-name>AttributeServlet2</servlet-name>
        <servlet-class>ServletAttr2</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>AttributeServlet2</servlet-name>
        <url-pattern>/servletAttr2</url-pattern>
    </servlet-mapping>
</web-app>
~~


