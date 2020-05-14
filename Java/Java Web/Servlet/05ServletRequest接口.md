# ServletRequest接口
`ServletReques`t的对象用于向Servlet提供客户端请求信息，如内容类型，内容长度，参数名称和值，标题信息，属性等。

# ServletRequest接口的方法
编号|方法|描述
--|--|--
1|`public String getParameter(String name)`|用于通过名称获取参数的值。
2|`public String[] getParameterValues(String name)`|返回一个包含给定参数名称的所有值的String数组。它主要用于获取多选列表框的值。
3|`java.util.Enumeration getParameterNames()`|返回所有请求参数名称的枚举。
4|`public int getContentLength()`|返回请求实体数据的大小，如果未知则返回-1。
5|`public String getCharacterEncoding()`|返回此请求输入的字符集编码。
6|`public String getContentType()`|返回请求实体数据的网络媒体类型，如果未知则返回null。
7|`public ServletInputStream getInputStream() throws IOException`|返回用于读取请求正文中二进制数据的输入流。
8|`public abstract String getServerName()`|返回接收请求的服务器的主机名。
9|`public int getServerPort()`|返回接收到此请求的端口号。

# ServletRequest显示用户名称的示例
在这个例子中，在servlet中显示用户提交上来的名字。这里使用`getParameter()`方法返回指定请求参数名称的值。
**ServletRequest.java**
~~~java
public class ServletRequest extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        // 设置响应内容类型器
        response.setContentType("text/html");
        // 获取响应输出对象
        PrintWriter out = response.getWriter();

        // 获取请求的内容
        String name = request.getParameter("name");
        if (name == null || name == ""){
            name = "";
        }

        out.println("Welcome "+name);
        out.close();
    }

}
~~~
**index.html**
~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Welcome  ServletRequest</title>
</head>
<body>
 <div style="text-align: center">
     <form action="ServletRequest/welcome">
         名字:<input type="text" name="name"><input type="submit" value="提交">
     </form>
 </div>
</body>
</html>
~~~
**web.xml**
~~~xml
<?xml version="1.0" encoding="UTF-8"?>

<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         id="WebApp_ID" version="3.1">
    <display-name>ServletRequest</display-name>
    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>/
    <servlet>
        <servlet-name>ServletRequest</servlet-name>
        <servlet-class>ServletRequest</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>ServletRequest</servlet-name>
        <url-pattern>/ServletRequest/welcome</url-pattern>
    </servlet-mapping>
</web-app>
~~~

# Servlet显示所有头信息
`ServletRequest`接口的`getHeaderNames()`方法返回一个包含所有头名称的`Enumeration`对象。`ServletRequest`接口的`getHeader()`方法返回给定头名称的头值。 在这个例子中，我们在servlet页面中显示一个请求的所有头信息。
`getHeaderNames()`方法的语法
~~~java
public Enumeration getHeaderNames()
~~~
`getHeader()`方法的语法
~~~java
public String getHeader(String headerName)
~~~

HeaderServlet.java
~~~java
public class HeaderServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        // 设置响应内容类型器
        response.setContentType("text/html");
        // 获取响应输出对象
        PrintWriter out = response.getWriter();

        Enumeration enums = request.getHeaderNames();
        while(enums.hasMoreElements()){
            String headName = (String)enums.nextElement();
            String headValue = request.getHeader(headName);
            out.println("<b>"+headName+"</b>");
            out.println(headValue+"</br>");
        }
    }
}
~~~
head.html
~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Show Heads Servlet</title>
</head>
<body>
    <div style="text-align: center">
        请<a href="ShowHeaders/headers?key1=name">点击这里</a>查看所有报头信息
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
        <welcome-file>head.html</welcome-file>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
    <servlet>
        <servlet-name>HeaderServlet</servlet-name>
        <servlet-class>HeaderServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>HeaderServlet</servlet-name>
        <url-pattern>/ShowHeaders/headers</url-pattern>
    </servlet-mapping>
</web-app>
~~~