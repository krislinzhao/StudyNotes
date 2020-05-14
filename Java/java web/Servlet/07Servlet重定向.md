`HttpServletResponse`接口的`sendRedirect()`方法可以用于将响应重定向到另一个资源，资源可能是`servlet`，`jsp`或`html`文件。
它接受相对和绝对URL。
它在客户端工作，因为它使用浏览器的URL栏来发出另一个请求。 所以，它可以在服务器内部和外部工作。

**forward()和sendRedirect()方法的区别**
`RequestDispatche`r的`forward()`方法和`HttpServletResponse`接口的`sendRedirect()`方法之间存在很多差异。如下面给出：
forward()方法|sendRedirect()方法
--|--|
`forward()`方法在服务器端工作。|`sendRedirect()`方法在客户端工作。
它将相同的请求和响应对象发送到另一个servlet。|它总是发送一个新的请求。
它只能在服务器内工作。|它可以在服务器内外使用。
示例: `request.getRequestDispacher("servlet2").forward(request,response);`|示例: `response.sendRedirect("servlet2");`

**`sendRedirect()`方法的语法**
~~~java
public void sendRedirect(String URL)throws IOException;
~~~
**`sendRedirect()`方法的示例**
~~~java
response.sendRedirect("http://www.baidu.com");
~~~

# sendRedirect()方法的示例

RedirectServlet.java
~~~java
public class RedirectServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        // 设置服务器端内容类型器
        response.setContentType("text/html");
        // 获取服务器端输出对象
        PrintWriter out = response.getWriter();

        // 直接重定向到：www.baidu.com
        response.sendRedirect("https://www.baidu.com");

        out.close();
    }
}
~~~

BaiduSearch.java
~~~java
public class BaiduSearch extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        // 获取用户输入的关键字
        String keyword =request.getParameter("keyword");
        System.out.println(keyword);
        String url = "https://www.baidu.com/s?ie=utf-8&f=3&rsv_bp=1&rsv_idx=1&ch=&tn=baidu&bar=&wd="+keyword+"&oq=serious&rsv_pq=b7e075bf00169b14&rsv_t=6c67hEJVKkO%2Bkg08XTXPh9dlymb7lzNfD9TVjJHyHFxBgPqqSGuCNRywm30&rqlang=cn&rsv_enter=1&prefixsug=%25E4%25BD%25A0%25E5%25A5%25BD&rsp=1&rsv_dl=ts_1&inputT=8774";
        System.out.println(url);
        // 重定向的百度搜索
        response.sendRedirect(url);
    }
}
~~~

redirect.html
~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Search by KeyWord</title>
</head>
<body>
    <div style="text-align: center">
        <form action="BaiduSearch">
            关键字:<input type="text" name="keyword"><input type="submit" value="百度搜索">
        </form>
    </div>
</body>
</html>
~~~

web.xml
~~~xml
?xml version="1.0" encoding="UTF-8"?>

<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         id="WebApp_ID" version="3.1">
    <display-name>ServletRequest</display-name>
    <welcome-file-list>
        <welcome-file>redirect.html</welcome-file>
    </welcome-file-list>
    <servlet>
        <servlet-name>RedirectServlet</servlet-name>
        <servlet-class>RedirectServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>RedirectServlet</servlet-name>
        <url-pattern>/RedirectServlet</url-pattern>
    </servlet-mapping>
    <servlet>
        <servlet-name>BaiduSearch</servlet-name>
        <servlet-class>BaiduSearch</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>BaiduSearch</servlet-name>
        <url-pattern>/BaiduSearch</url-pattern>
    </servlet-mapping>
</web-app>
~~~