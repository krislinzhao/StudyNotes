# welcome-file-list
在`web.xml`文件中的`web-app`块的`welcome-file-list`子元素用于定义欢迎文件列表。 它的子元素是`welcome-file`，用于定义欢迎文件(即默认打开的页面)。
欢迎文件是服务器自动调用的文件，如果不指定任何文件名。
默认情况下，服务器按以下顺序查找欢迎文件：

 1. web.xml文件中的`welcome-file-list`指定的文件
 2. index.html
 3. index.html
 4. index.jsp

如果没有找到这些文件，服务器会报告404错误。

如果在`web.xml`中指定了`welcome-file`，并且所有文件`index.html`，`index.html`和`index.jsp`都存在，那么优先级将转到`welcome-file`。
如果`web.xml`文件中不存在`welcome-file-list`项，那么优先级到`index.html`文件，然后是`index.html`，以及最后是`index.jsp`文件。
下面来看看一个定义欢迎文件的web.xml文件。
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://xmlns.jcp.org/xml/ns/javaee"
    xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
    id="WebApp_ID" version="3.1">
    <display-name>helloworld</display-name>
    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.jsp</welcome-file>
        <welcome-file>home.jsp</welcome-file>
    </welcome-file-list>
</web-app>
~~~
现在，`index.html`，`index.jsp`和`home.jsp`将是欢迎文件。
如果有欢迎文件，可以按如下所示的方式调用项目：
~~~shell
http://localhost:8888/helloproject
~~~
如上所示，我们并没有在项目名称(`helloproject`)之后指定任何文件名。上面URL访问相当于以下三个 
~~~shell
http://localhost:8888/helloproject/index.html
http://localhost:8888/helloproject/index.jsp
http://localhost:8888/helloproject/home.jsp
~~~

# Servlet启动时加载
## 在web.xml文件中指定启动时加载
如果`load-on-startup`元素值为正，则会在Web应用程序部署或服务器启动时加载servlet。 它也被称为servlet的预初始化。
可以指定传递servlet的值(`load-on-startup`元素指定的值)为正或为负。
## load-on-startup元素的优点
servlet在第一个请求时被加载。这意味着它会在第一次请求时消耗更多的时间。 如果在`web.xml`中指定启动加载，则servlet将在项目部署时间或服务器启动时加载。 所以，响应第一个请求需要较少的时间。
下面来看一个简单的`web.xml`配置`load-on-startup`元素的示例代码 
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://xmlns.jcp.org/xml/ns/javaee"
    xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
    id="WebApp_ID" version="3.1">
    <display-name>helloworld</display-name>
    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.jsp</welcome-file>
        <welcome-file>home.jsp</welcome-file>
    </welcome-file-list>
    <servlet>
        <servlet-name>MyServlet1</servlet-name>
        <servlet-class>MyServlet</servlet-class>
        <load-on-startup>0</load-on-startup>  
    </servlet>
    <servlet>
        <servlet-name>BServlet</servlet-name>
        <servlet-class>BServlet</servlet-class>
        <load-on-startup>1</load-on-startup>  
    </servlet>

    <servlet-mapping>
        <servlet-name>MyServlet</servlet-name>
        <url-pattern>/index</url-pattern>
    </servlet-mapping>
</web-app>
~~~
定义了**2**个servlet，这两个servlet将在项目部署或服务器启动时加载。但是，首先将`MyServlet1`加载，然后再加载`BServlet`。

### 传递负值
如果传递`load-on-startup`元素为负值，则此servlet将请求时第一个加载。
