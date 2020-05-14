`FilterConfig`的对象由Web容器创建。这个对象可用于获取`web.xml`文件中的配置信息。

# FilterConfig接口的方法
`FilterConfig`接口中有以下4个方法。

* `public void init(FilterConfig config)`:` init()`方法仅在初始化过滤器时被调用(只调用一次)
* `public String getInitParameter(String parameterName)`: 返回指定参数名称的参数值。
* `public java.util.Enumeration getInitParameterNames()`: 返回包含所有参数名称的枚举。
* `public ServletContext getServletContext()`: 返回ServletContext对象。

# `FilterConfig`示例
在此示例中，如果将`web.xml`的中的`construction`参数值为`no`，则请求将转发到servlet，如果将参数值设置为：`yes`，则过滤器将使用消息创建响应：”此页面正在处理中“。下面来看看FilterConfig的简单例子。

以下是这个项目中的几个主要的代码文件。在这里创建了**4**个文件：

* FilterConfig.html - 应用程序入口
* MyFilterConfig.java - 过滤器实现
* HelloServlet.java - 一个简单的Servlet
* web.xml - 项目部署配置文件/

MyFilterConfig.java
~~~java
public class MyFilterConfig implements Filter {
    FilterConfig config;
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        this.config = filterConfig;
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        // 设置服务器端的内容类型
        servletResponse.setContentType("text/html;charset=utf-8");
        // 设置服务器端的编码
        servletResponse.setCharacterEncoding("utf-8");
        // 设置客户端的编码
        servletRequest.setCharacterEncoding("utf-8");
        // 获取服务器端的输出对象
        PrintWriter out = servletResponse.getWriter();

        // 获取配置信息
        String s = config.getInitParameter("construction");

        if (s.equals("yes")){
            out.println("此页面正在处理");
        }else {
            out.println("过滤之前");
            filterChain.doFilter(servletRequest,servletResponse);
            out.println("过滤之后");
        }
    }

    @Override
    public void destroy() {

    }
}
~~~
HelloServlet.java
~~~java
public class HelloServlet extends HttpServlet {
    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        // 设置服务器端内容类型
        response.setContentType("text/html;charset=utf-8");
        // 设置服务器端编码
        response.setCharacterEncoding("utf-8");
        // 设置客户端编码
        request.setCharacterEncoding("utf-8");
        // 获取服务器端输出对象
        PrintWriter out = response.getWriter();

        out.println("<b>Welcome to HelloServlet</b>");
    }
}
~~~
FilterConfig.html
~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>FilterConfig示例</title>
</head>
<body>
    <div style="text-align: center">
        <a href="helloServlet">查看Filter配置信息</a>
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
        <welcome-file>FilterConfig.html</welcome-file>
    </welcome-file-list>
    <servlet>
        <servlet-name>HelloServlet</servlet-name>
        <servlet-class>HeaderServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>HelloServlet</servlet-name>
        <url-pattern>/helloServlet</url-pattern>
    </servlet-mapping>
    <filter>
        <filter-name>MyFilterConfig</filter-name>
        <filter-class>MyFilterConfig</filter-class>
        <init-param>
            <param-name>construction</param-name>
            <param-value>no</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>MyFilterConfig</filter-name>
        <url-pattern>/helloServlet</url-pattern>
    </filter-mapping>
<web-app>
~~~