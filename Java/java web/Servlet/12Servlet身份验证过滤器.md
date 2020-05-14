# 使用过滤器验证用户的示例
下面来看看如何使用过滤器验证用户的简单示例。
在这个示例中创建了以下几个主要的代码文件：

* Authentication.html - 首页
* MyAuthenticationFilter.java - 过滤器，用于处理用户登录信息和跳转。
* AdminServlet.java - 管理员的Servlet
* web.xml - 项目描述符和配置信息。

AdminServlet.java
~~~java
public class AdminServlet extends HttpServlet {
    @Override
    public void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException {
        // 设置服务器端内容类型
        response.setContentType("text/html;charset=utf-8");
        // 设置服务器端编码
        response.setCharacterEncoding("utf-8");
        // 设置客户端编码
        request.setCharacterEncoding("utf-8");
        // 获取服务器端输出对象
        PrintWriter out = response.getWriter();

        out.println("欢迎来到 Admin页面");
        out.close();
    }
}
~~~
MyAtuthenticationFilter.java
~~~java
public class MyAuthenticationFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("Filter init");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        // 设置服务器端内容类型
        servletResponse.setContentType("text/html;charset=utf-8");
        // 设置服务器端编码
        servletResponse.setCharacterEncoding("utf-8");
        // 设置客户端编码
        servletRequest.setCharacterEncoding("utf-8");
        // 获取服务器端输出对象
        PrintWriter out = servletResponse.getWriter();

        //获取密码
        String password = servletRequest.getParameter("password");
        if (password == null){
            password = "";
        }

        if (password.equals("admin")){
            filterChain.doFilter(servletRequest,servletResponse);
        }
        else {
            // include的请求转发
            out.println("密码错误");
            RequestDispatcher dispatcher = servletRequest.getRequestDispatcher("Authentication.html");
            dispatcher.include(servletRequest,servletResponse);
        }
    }

    @Override
    public void destroy() {
        System.out.println("Filter destroy");
    }
}
~~~
Authentication.html
~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>使用过滤器验证用户</title>
</head>
<body>
    <div style="text-align: center">
        <form action="login" method="post">
            用户名:<input type="text" name="userName" value="krislin"><br/>
            密码:<input type="password" name="password"><br/>
            <input type="submit" value="登录">
        </form>
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
        <welcome-file>Authentication.html</welcome-file>
        <welcome-file>Filter.html</welcome-file>
        <welcome-file>Context.html</welcome-file>
        <welcome-file>dbConfig.html</welcome-file>
        <welcome-file>redirect.html</welcome-file>
        <welcome-file>forward.html</welcome-file>
        <welcome-file>head.html</welcome-file>
    </welcome-file-list>
    <servlet>
        <servlet-name>AdminServlet</servlet-name>
        <servlet-class>AdminServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>AdminServlet</servlet-name>
        <url-pattern>/login</url-pattern>
    </servlet-mapping>
    <filter>
        <filter-name>MyAuthentication</filter-name>
        <filter-class>MyAuthenticationFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>MyAuthentication</filter-name>
        <url-pattern>/login</url-pattern>
    </filter-mapping>
<web-app>
~~~

