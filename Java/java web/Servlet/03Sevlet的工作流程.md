# Servlet是如何工作的？
了解servlet如何工作对了解servlet内部工作流程很重要。 在这里，我们将以第一个servlet程序来讲解它的内部细节。

**服务器检查servlet是否为第一次被请求？**
如果**是第一次**被请求，则 - 

* 加载servlet类。
* 实例化servlet类。
* 调用init方法传递ServletConfig对象
  
如果**不是第一次**被请求，则 - 

* 调用service方法传递请求和响应对象
  
Web容器在需要删除servlet时调用`destroy`方法，例如：在停止服务器或取消部署项目时。

# Web容器如何处理servlet请求？
Web容器负责处理请求。下面来看看看它如何处理请求。

* 将请求与web.xml文件中的servlet进行映射。
* 为请求创建请求和响应对象
* 调用线程上的`service`方法
* 公共`service`方法内部调用受保护的`service`方法
* 受保护的`service`方法根据请求的类型调用`doGet`方法。
* `doGet`方法生成响应，并将其传递给客户端。
* 发送响应后，Web容器将删除请求和响应对象。线程包含在线程池中或删除取决于服务器实现。
  
# 在公共service方法中写什么？
公共`service`方法将`ServletRequest`对象转换为`HttpServletRequest`类型和`ServletResponse`对象转为`HttpServletResponse`类型。然后调用传递这些对象的`service`方法。下面来看看内部代码：
~~~java
public void service(ServletRequest req, ServletResponse res)  
        throws ServletException, IOException  
{  
        HttpServletRequest request;  
        HttpServletResponse response;  
        try  
        {  
            request = (HttpServletRequest)req;  
            response = (HttpServletResponse)res;  
        }  
        catch(ClassCastException e)  
        {  
            throw new ServletException("non-HTTP request or response");  
        }  
        service(request, response);  
}
~~~
# 在受保护的service方法中编写什么？
受保护的`service`方法检查请求的类型，如果请求类型为`get`，则调用`doGet`方法，如果请求类型为`post`，则调用`doPost`方法。下面来看看内部代码：
~~~java
protected void service(HttpServletRequest req, HttpServletResponse resp)  
        throws ServletException, IOException  
{  
    String method = req.getMethod();  
    if(method.equals("GET"))  
    {  
        long lastModified = getLastModified(req);  
        if(lastModified == -1L)  
        {  
            doGet(req, resp);  
        }   
        ....  
        //rest of the code  
    }  
}
~~~

