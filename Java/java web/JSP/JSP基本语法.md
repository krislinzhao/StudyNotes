# JSP 声明
`<%! declaration; [ declaration; ]+ ... %>`
下面是简单的例子JSP声明：
~~~jsp
<%! int i = 0; %> 
<%! int a, b, c; %> 
<%! Circle a = new Circle(2.0); %> 
~~~

# JSP表达式

	JSP表达式元素包含计算，转换为字符串，并插入出现在JSP文件的脚本语言表达式。

	因为一个表达式的值被转换为字符串，可以在文本一行内使用表达式，不管它是否被标记使用在HTML，JSP文件中。

	表达元素可以包含任何Java语言规范有效的表达式，但是不能使用一个分号来结束表达式。

下面是JSP表达式的语法：
`<%= expression %>`


下面是简单的例子JSP表达式：
~~~jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<html>
	<head>
		<title>JSP表达式 </title>
	</head>
	<body>
		<p>
			今天是:<%=(new java.util.Date()).toLocaleString()%>
		</p>
	</body>
</html>
~~~

# JSP注释

	JSP注释标记的文字或语句都会被JSP容器忽略。当想要隐藏或“注释掉”JSP页面的一部分，JSP注释是很有用的。

以下是JSP注释语法：
`<%-- This is JSP comment --%>`

JSP注释示例：
~~~jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<html> 
<head><title>注释 - 示例</title></head>  
<body> 
<h2>A Test of Comments</h2> 
<%-- This comment will not be visible in the page source --%> 
</body> 
</html> 
~~~

还有少数特殊的结构可以使用一些情况，插入注释或字符，将被特殊处理。这里有一个整理汇总：
 语法|目的|
--|--
<%-- comment --%>|    JSP注释，它将被JSP引擎忽略
\<!-- comment --> |    HTML注释，它将被浏览器忽略
 <\%|表示静态<%的字面量
 %\>|    表示静态%>的字面量
\\' |    在使用单引号在属性中的单引号
   \ \"|    双引号在属性使用双引号

# JSP 指令
		
JSP指令影响的servlet类的整体结构。它通常有以下形式：
`<%@ directive attribute="value" %>`
		
有三种类型的指令标记：
指令|描述
--|--
`<%@ page ... %>`|定义页面依赖属性，例如脚本语言，错误页面和缓冲的要求
`<%@ include ... %>`|包括在转换阶段的文件
`<%@ taglib ... %>`|声明了一个标签库，包含自定义动作，用在页面中

# JSP 动作
		
    JSP动作使用XML语法结构来控制Servlet引擎的行为。可以动态地插入文件，重用JavaBeans组件，用户转发到另一个页面，或为Java插件生成HTML。
		
只有一个用于动作元素的语法，因为它符合XML标准：
`<jsp:action_name attribute="value" />`
		
动作元素基本上都是预先定义函数并有以下可用的JSP操作：
语法|目的
--|--
`jsp:include`|包括页面被一次请求的文件
`jsp:include`|包括页面被一次请求的文件
`jsp:useBean`|查找或实例化一个`JavaBean`
`jsp:setProperty`|设置一个`JavaBean`的属性
`jsp:getProperty`|插入一个`JavaBean`的属性到输出
`jsp:forward`|转发请求到一个新的页面
`jsp:plugin`|生成特定浏览器的代码，使对象或嵌入标签Java插件
`jsp:element`|定义XML元素动态
`jsp:attribute`|定义动态定义XML元素的属性
`jsp:body`|定义动态定义的XML元素主体
`jsp:text`|用于编写模板文本在JSP页面和文档


		
# JSP 隐式对象：

JSP支持九种自动定义的变量，这也被称为隐式对象。这些变量是：

对象|描述
--|--
`request`|这是与请求相关联的HttpServletRequest对象
`response`|这是用于响应客户端相关联的HttpServletResponse对象
`out`|这是用于将输出发送给客户端的PrintWriter对象
`session`|这是与请求相关联的HttpSession对象
`application`|这是应用程序上下文关联的ServletContext对象
`config`|这是与页面关联的ServletConfig对象
`pageContext`|这封装采用类似更高的性能JspWriters服务器特定的功能
`page`|这是一个简单的代名词，是用来调用由转换servlet类中定义的方法
`Exception`|`Exception`对象允许例外的数据由JSP指定访问


# 控制流语句

JSP提供了Java的全部功能可以嵌入在Web应用程序。可以使用Java的所有API和构建块在JSP编程，包括决策语句，循环等。

## 决策声明

在`if ... else`块开头时就像一个普通的Scriptlet，但 Scriptlet 每一行是封闭的，包括scriptlet标记之间的HTML文本。创建一个JSP文件为：`if-else.jsp`，并写入以下代码：
~~~jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%! int day = 3; %> 
<html> 
<head><title>IF...ELSE 示例</title></head> 
<body>
<% if (day == 1 | day == 7) { %>
<p> Today is weekend</p>
<% } else { %>
<p> Today is not weekend</p>
<% } %>
</body> 
</html>
~~~

`switch...case` 块，编写通过使用`out.println()`和内小脚本有一点不同，把下面代码保存到` switch-case.jsp`文件中，代码详细如下：
~~~jsp
<%! int day = 3; %> 
<html> 
<head><title>SWITCH...CASE 示例</title></head> 
<body>
<% 
switch(day) {
case 0:
   out.println("It\'s Sunday.");
   break;
case 1:
   out.println("It\'s Monday.");
   break;
case 2:
   out.println("It\'s Tuesday.");
   break;
case 3:
   out.println("It\'s Wednesday.");
   break;
case 4:
   out.println("It\'s Thursday.");
   break;
case 5:
   out.println("It\'s Friday.");
   break;
default:
   out.println("It's Saturday.");
}
%>
</body> 
</html>
~~~

# 循环语句
		
还可以使用在Java中三种基本循环类型块：`for`, `while`,and `do.while `块在JSP编程中。

让我们来看看下面的`for`循环示例，把下面代码保存到 loop-for.jsp 文件中，代码详细如下：
~~~jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%! int fontSize; %> 
<html> 
<head><title>FOR 循环示例</title></head> 
<body>
<%for ( fontSize = 1; fontSize <= 3; fontSize++){ %>
   <font color="green" size="<%= fontSize %>">
    JSP Tutorial
   </font><br />
<%}%>
</body> 
</html> 
~~~
`while `循环编写，把下面代码保存到 loop-while.jsp 文件中，代码详细如下：
~~~jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%! int fontSize; %> 
<html> 
<head><title>WHILE循环示例</title></head> 
<body>
<h2>While循环示例：</h2>
<%while ( fontSize <= 5){ %>
   <font color="green" size="<%= fontSize %>">
    JSP Tutorial
   </font><br />
<%fontSize++;%>
<%}%>
</body> 
</html>
~~~

# JSP 运算符
		
JSP支持所有支持Java的逻辑和算术运算符。下表给出了所有的运算符，具有最高优先级将排在表的顶部， 运算级别最低的放在底部。

在一个表达式中，具有更高的优先级运算符将首先计算评估。

分类|操作符|关联 
--|--|--
后缀|() [] . (点运算符)|左->右
一元|++ - - ! ~|右->左
乘法|* / % |左->右
加法|+ - |左->右
位移|>> >>> <<  |左->右
关系|> >= < <=  |左->右 
相等|== != |左->右
位与/AND |& |左->右 
位XOR |^ |左->右
位OR | \| |左->右
逻辑AND |&& |左->右
逻辑OR | \| \| |左->右 
关系|?: |右->左
赋值|= += -= *= /= %= >>= <<= &= ^= \|= |右->左
逗号|, |左->右

# JSP 字面量

JSP表达式语言定义了以下字面量：

 * Boolean: true 或 false
*  Integer: 与Java中的一样
*  Float: 与Java中的一样
*   String: 单引号和双引号; " 转义为 \"。' 转义为 \'， 以及 \ 转义为 \\
*   Null: null

# JSP对象范围

定义为一个 JSP 对象范围

说明
JSP对象所使用的可用性通常是由该对象的范围限定。

**Page 范围:**
使用此JSP对象可以在其中创建的页面内使用。

**Request 范围:**
使用该JSP对象可以在请求服务任何地方使用。

**Session 范围:**
使用该JSP的对象可用于在属于同一个会话页面。

**Application 范围:**
使用该JSP的对象可以在整个应用程序页面中使用。


将下面的代码编写到 scope1.jsp 文件，代码内容如下所示：
~~~jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<c:set var="Pag" value="Page Value" scope="page" />
<c:set var="Req" value="Request Value" scope="request" />
<c:set var="Ses" value="Session Value" scope="session" />
<c:set var="App" value="Application Value" scope="application" />
<html>
<body>
    <h2>JSP对象范围示例一：</h2>
    <b>Page Scope</b> ::<c:out value="${Pag}" /><br>
    <b>Request Scope</b> ::<c:out value="${Req}" /><br>
    <b>Session Scope</b> ::<c:out value="${Ses}" /><br>
    <b>Application Scope</b>::<c:out value="${App}" /><br>
    <a href="scope2.jsp">下一页Session,Application范围</a>
</body>
</html>
~~~

# 在JSP页面中创建方法
方法即是可以在一个JSP页面被用于执行特定操作的一个代码段。

创建一个方法示例：
~~~jsp
<%!
public int mul(int a, int b){
    return a * b;
}
%>
~~~
两个数相乘的结果是：`<%= mul(2, 2) %>`

以上代码保存，并执行结果如下：
  两个数相乘的结果是：`4`
 
另外，在上述例子中使用的方法是：mul，将返回一个整数值作为输出。 它需要两个整数“a”，“b”作为参数，以产生两个数字的乘积作为输出。

# 在JSP页面中使用数组

由于JSP不是一个完整的编程语言不具有数组的声明，但使用Java中的数据结构在JSP中使用是完全没有问题的。

~~~jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
String[] arr={"apple","orange","cherry"};
%>
<%
int j;
out.println("<p>数组中所有元素是：</p>");
for(j=0;j<arr.length;j++){
    out.println(arr[j]);
}
%>
~~~

# 在JSP中使用Java Bean
	
Java bean只不过是一个实现`java.io.Serializable`接口，并且使用`set/get`方法来投射类的属性。因为它们是一个Java bean类的实例并可重复使用，在JSP页面中提供了灵活性，。
嵌入一个Java bean到JSP网页，有三个基本动作或标签：`<jsp:useBean>`, `<jsp:setProperty>`, `<jsp:getProperty>`

1. <jsp:useBean>
这个标签是用来给bean指定“id”和“scope”属性相关联。

2. <jsp:setProperty>
这个标签被用于设置一个beans属性的值，主要使用“name”属性已经定义了相同的范围对象。其他属性是 "property", "param", "value"

3. <jsp:getProperty>
这个标签是用来获取引用Bean实例属性并将其存储到隐式out对象。

## Beans的规则：

* 包应该是java bean的第一行
*  Bean应该有一个空的构造
*  所有的bean中的变量应该设置有“get”，“set”方法。
*  属性名应以大写字母开头在使用“set”，“get”方法时。
*  例如变量“名称”的get，set方法就是getName(), setName(String)
*  设置方法应该返回像一个空(void)值： "return void()"

bean.java
~~~java
package bean;
public class Counter
{
int count = 0;
String name;
public Counter() { }
public int getCount()
{
    return this.count;
}
public void setCount(int count){
    this.count = count;
} 
public String getName(){
    return this.name; 
} 
public void setName(String name){
    this.name=name; 
}
}
~~~
use-bean.jsp
~~~jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page language="java" %>
<%@ page import="bean.Counter" %>
</body>
<jsp:useBean id="counter" scope="page" class="bean.Counter" />
<jsp:setProperty name="counter" property="count" value="4" />
Get Value: <jsp:getProperty name="counter" property="count" /><BR>
<jsp:setProperty name="counter" property="name" value="baidu.com" />
Get Name: <jsp:getProperty name="counter" property="name" /><BR>
~~~
使用`<jsp:useBean>`动作包含`bean.Counter`这个`bean`，并设置此`bean`的`id`为`counter`,范围是`page`.使用`<jsp:setProperty>`设置`id=counter`的`bean`的`count`属性 的值为 `4` ，以及设置` name`的值为 `baidu.com.` 并使用 `<jsp:getProperty>` 来获取它们的值。

# JSP Session

会话处理变得必不可少，当一个请求数据需要被持续保持以供进一步使用。 由于HTTP协议认为每个请求是一个新的请求，它不能保持过去访问的数据，因此会话处理就变得很重要。以下是一些来处理会话的方法。
* JSP中每当发起一个请求，服务器产生一个存储在客户机的唯一会话ID。
* Cookies存储信息在客户端浏览器
* URL重写会话信息附加到URL的末尾
* 隐藏的表单域将SessionID嵌入到GET和POST命令。

创建一个页面 session.jsp，代码如下：
~~~jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<html>
<head><title>Session示例</title></head>
<body>
<h2>Session示例：</h2> 
<form method = "post" action="session2.jsp">
<font>Username<input type = "text" name = "name"></font>
</font><br><br>
<input type = "submit" name = "submit" value = "提交" >
</form>
</body>
</html>
~~~

创建另一个页面 session2.jsp，代码如下：
~~~jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<html>

<head><title>Session示例2</title></head>

<body> 
<%
String name = request.getParameter("name");
if((name!=null))
{
    session.setAttribute("username",name);
}
%>
<a href="session3.jsp">继续，跳转到session3.jsp</a>
~~~


创建另一个页面 session3.jsp，代码如下：
~~~jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<html>
<head><title>Session示例3</title></head> 
<body>
<font>欢迎您，</font> <%= session.getAttribute("username") %>
</body>
</html>
~~~
	
在第一个示例中的 "session1.jsp" 是用来提供一个表单以获得用户名。 当提交表单时它转到第二个文件session2.jsp，它调用表单的“action”属性。一个Session的属性使用 "session.setAttribute"设置在这里. 在第三个文件 "session3.jsp" 相同的值使用"session.getAttribute" 来显示出来。

# JSP Cookies信息处理

`Cookies`和会话有一些相似的地方，唯一的区别是`cookies` 存储在浏览器。而会话存储在服务器端。让我们来看看在JSP中处理`Cookies`的一些例子：

示例代码: cookie1.jsp
~~~jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<html>
<body>
<form method = "post" action="cookie2.jsp">
<font>Username<input type = "text" name = "name"></font>
</font><br>
<input type = "submit" name = "submit" value = "submit" >
</form>
</body>
</html>
 ~~~

示例代码: cookie2.jsp
~~~jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page language="java" import="java.util.*"%>
<%
String name=request.getParameter("name");
Cookie cookie = new Cookie ("name",name);
response.addCookie(cookie);
cookie.setMaxAge(50 * 50); //Time is in Minutes
%>
<a href="cookie3.jsp">Continue</a>
~~~

示例代码: cookie3.jsp
~~~jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<p>显示Cookie的值：</p>
<%
Cookie[] cookies = request.getCookies();
for (int i=0; i<cookies.length; i++){
    if(cookies[i].getName().equals("name"))
        out.println("Hello"+cookies[i].getValue());
}
%>
~~~

在第一个例子`cookie1.jsp`中，使用表单得到用户名。当表单提交进入第二页`cookie2.jsp`其中Cookie使用`cookie.setMaxAge`函数设置过期时间。在第三个页面`cookie3.jsp`中，`cookie`使用函数`request.getCookies()`，并使用循环是得到了字段`username`的值并显示。

# 删除Cookies	
示例代码: cookie4.jsp
~~~jsp
<%
Cookie cookie = new Cookie( "name", "" );
cookie.setMaxAge( 0 );
%>
~~~
另外，在上述例子中，我们已经创建的 `cookie`的新实例并使用一个`null`值并将`cookie`的`age`设定为`0`。 这将删除`cookie`。

