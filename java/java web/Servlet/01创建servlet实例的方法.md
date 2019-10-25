# 1.通过实现Servlet接口的Servlet实例
需要实现`Servlet`接口来创建任何`servlet`(直接或间接)。它提供了**3**个生命周期方法，用于初始化`servlet`，服务请求以及销毁`servlet`和**2**个非生命周期方法。
## Servlet接口的方法
方法|描述|
---------|-------|
`public void init(ServletConfig config)`|初始化servlet它是servlet的生命周期方法，由web容器调用一次。
`public void service (ServletRequest request, ServletResponse response)`|为传入的请求提供响应它由Web容器的每个请求调用。
`public void destroy()`|仅被调用一次，并且表明servlet正在被销毁。
`public ServletConfig getServletConfig()`|返回ServletConfig对象。
`public String getServletInfo()`|返回有关servlet的信息，如作者，版权，版本等
~~~java
public class MyServlet implements Servlet {
    ServletConfig servletConfig = null;

    /**
     * 初始化servlet，它是servlet的生命周期方法，由web容器调用一次。
     * @param servletConfig servlet的配置
     */
    @Override
    public void init(ServletConfig servletConfig){
        this.servletConfig = servletConfig;
        System.out.println("servlet is initialized");
    }

    /**
     * 为传入的请求提供响应。它由Web容器的每个请求调用。
     * @param request 请求
     * @param response 响应
     * @throws IOException
     * @throws ServletException
     */
    @Override
    public void service(ServletRequest request, ServletResponse response) throws IOException, ServletException {
        // 设置响应内容类型器
        response.setContentType("text/html");
        // 获取响应输出对象
        PrintWriter out = response.getWriter();

        out.print("<html><body>");
        out.print("<div style='text-align:center;'><h1>Hello Simple Servlet</h1></div>");
        out.print("</body></html>");
    }

    /**
     * 仅被调用一次，并且表明servlet正在被销毁。
     */
    @Override
    public void destroy(){
        System.out.println("servlet is destroyed");
    }

    /**
     * 返回ServletConfig对象。
     * @return 返回servlet的配置对象
     */
    @Override
    public ServletConfig getServletConfig(){
        return servletConfig;
    }

    /**
     *返回有关servlet的信息，如作者，版权，版本等。
     * @return 返回servlet的信息
     */
    @Override
    public String getServletInfo(){
        return "copyright 2012-2020";
    }
}
~~~
# 2.通过继承GenericServlet类的Servlet实例
`GenericServlet`类实现了`Servlet`，`ServletConfig`和`Serializable`接口。它提供除了`service`方法之外，还实现了这些接口的所有方法。`GenericServlet`类可以处理任何类型的请求，因此它与协议无关。可以通过继承`GenericServlet`类并提供服务方法的实现来创建通用的`servlet`。

## GenericServlet类的方法
序号|方法|描述
--|--|--
1|`public void init(ServletConfig config)`|用于初始化servlet
2|`public abstract void service(ServletRequest request, ServletResponse response)`|为传入请求提供服务，每当用户请求一个servlet时调用它。
3|`public void destroy()`|在整个生命周期中仅调用一次，以表明servlet正在被销毁。
4|`public ServletConfig getServletConfig()`|返回ServletConfig对象
5|`public String getServletInfo()`|返回有关servlet的信息，如作者，版权，版本等。
6|`public void init()`|这是servlet程序员的一个方便的方法，现在不需要调用super.init(config)
7|`public ServletContext getServletContext()`|返回ServletContext的对象。
8|`public String getInitParameter(String name)`|返回给定参数名称的参数值。
9|`public Enumeration getInitParameterNames()`|返回web.xml文件中定义的所有参数。
10|`public String getServletName()`|返回servlet对象的名称。
11|`public void log(String msg)`|在servlet日志文件中写入给定的消息。
12|`public void log(String msg,Throwable t)`|将说明性消息写入servlet日志文件和堆栈跟踪。

~~~java
public class MyServlet1 extends GenericServlet {
    @Override
    public void service(ServletRequest request, ServletResponse response) throws IOException, ServletException {
        // 设置响应内容选择器
        response.setContentType("text/html");
        // 获取响应输出对象
        PrintWriter out = response.getWriter();
        out.print("<html><body>");
        out.print("<div style=\"text-align:center;\"><h1>Hello Generic Servlet</h1></div>");
        out.print("</body></html>");
    }
}
~~~

# 通过继承HttpServlet类的Servlet实例
## HttpServlet类的方法
`HttpServlet`类扩展了`GenericServlet`类并实现了`Serializable`接口。它提供了`http`特定的方法，如:`doGe`t，`doPost`，`doHead`，`doTrace`等.
序号|方法|描述
--|--|--
1|'public void service(ServletRequest req,ServletResponse res)`|通过将请求和响应对象转换为http类型将请求调度到受保护的service方法。
2|`protected void service(HttpServletRequest req, HttpServletResponse res)`|从service方法接收请求，并根据传入的http请求类型将请求发送到doXXX()方法。
3|`protected void doGet(HttpServletRequest req, HttpServletResponse res)`|处理GET请求，它由Web容器调用。
4|`protected void doPost(HttpServletRequest req, HttpServletResponse res)`|处理POST请求，它由Web容器调用。
5|`protected void doHead(HttpServletRequest req, HttpServletResponse res)`|处理HEAD请求，它由Web容器调用。
6|`protected void doOptions(HttpServletRequest req, HttpServletResponse res)`|处理OPTIONS请求，它由Web容器调用。
7|`protected void doPut(HttpServletRequest req, HttpServletResponse res)`|处理PUT请求，它由Web容器调用。
8|`protected void doTrace(HttpServletRequest req, HttpServletResponse res)`|处理TRACE请求，它由Web容器调用。
9|`protected void doDelete(HttpServletRequest req, HttpServletResponse res)`|处理DELETE请求，它由Web容器调用。
10|`protected long getLastModified(HttpServletRequest req)`|返回自1970年1月1日GMT以来HttpServletRequest上次修改的时间。

~~~java
public class HelloServlet extends HttpServlet {
    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response){

        try {
            response.getWriter().println("<h1>Hello Servlet!</h1>");
            response.getWriter().println(new Date().toString());
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
~~~

