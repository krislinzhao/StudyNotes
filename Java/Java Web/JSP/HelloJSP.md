~~~jsp
<%@page contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" import="java.util.*"%>
 
你好 JSP
 
<br>
 
<%=new Date().toLocaleString()%>
~~~
JSP的`<%@page`指令
~~~jsp
<%@page contentType="text/html; charset=UTF-8"  pageEncoding="UTF-8" import="java.util.*"%>
~~~

`contentType="text/html; charset=UTF-8" `相当于`contentType="text/html; charset=UTF-8"`;通知浏览器以UTF-8进行中文解码

`pageEncoding="UTF-8" ` 如果jsp文件中出现了中文，这些中文使用UTF-8进行编码

`import="java.util.* `
导入其他类，如果导入多个类，彼此用,逗号隔开，像这样 `import="java.util.*,java.sql.*"`

`<%=new Date().toLocaleString()%>`
输出当前时间，相当于在Servlet中使用`response.getWriter()`进行输出
`response.getWriter().println(new Date().toLocaleString());`