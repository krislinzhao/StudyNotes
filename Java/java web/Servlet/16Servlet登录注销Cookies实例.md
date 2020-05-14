在这个应用程序中，创建了以下文件。
* index.html - Web应用程序首页。
* link.html - 链接页面。
* login.html - 登录页面。
* LoginServlet.java - 登录Servlet处理。
* LogoutServlet.java - 注销Servlet处理。
* ProfileServlet.java - 用户个人资料Servlet。
* web.xml - Servlet配置文件。

index.html
~~~html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>使用Cookie登录应用程序</title>
</head>
<body style="text-algin: center;">
<h2>欢迎使用Cookie登录应用程序</h2>
<a href="login.html">登录</a>|
<a href="logout">注销</a>|
<a href="profile">个人信息</a>
</body>
</html>
~~~
link.html
~~~html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
</head>
<a href="login.html">登录</a>|
<a href="logout">注销</a>|
<a href="profile">个人信息</a>
<hr>
</html>
~~~
login.html
~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>登录</title>
</head>
<body>
<div style="text-algin: center; padding-top:12px;">
    <form action="login" method="post">
        用户名：<input type="text" name="name"> 密码：<input type="password"
                                                      name="password"><input type="submit" value="登录">
    </form>
</div>
</body>
</html>
~~~

ServletLogin.java
~~~java
@WebServlet(name = "ServletLogin")
public class ServletLogin extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=UTF-8");
        request.setCharacterEncoding("UTF-8");

        PrintWriter out = response.getWriter();
        request.getRequestDispatcher("link.html").include(request, response);

        String name = request.getParameter("name");
        String password = request.getParameter("password");

        // 用户名和密码分别为：admin , 123456
        if (name.equals("admin") && password.equals("123456")) {
            out.print("您已成功登录系统!");
            out.print("<br>欢迎您, " + name);
            Cookie ck = new Cookie("name", name);
            response.addCookie(ck);
        } else {
            out.print("<font style='color:red;'>用户名或密码错误!</font>");
            request.getRequestDispatcher("login.html").include(request, response);
        }
        out.close();
    }
}
~~~
ServletLogout.java
~~~java
@WebServlet(name = "ServletLogout")
public class ServletLogout extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=UTF-8");
        request.setCharacterEncoding("UTF-8");
        PrintWriter out = response.getWriter();

        request.getRequestDispatcher("link.html").include(request, response);

        Cookie ck = new Cookie("name", "");
        ck.setMaxAge(0);
        response.addCookie(ck);

        out.print("您已成功注销!");
    }
}
~~~
ServletProfile.java
~~~java
@WebServlet(name = "ServletProfile")
public class ServletProfile extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=UTF-8");
        request.setCharacterEncoding("UTF-8");
        PrintWriter out = response.getWriter();

        request.getRequestDispatcher("link.html").include(request, response);

        Cookie ck[] = request.getCookies();
        if (ck != null) {
            String name = ck[0].getValue();
            if (!name.equals("") || name != null) {
                out.print("<b>欢迎您来到个人信息中心</b>");
                out.print("<br>您好, " + name);
            }
        } else {
            out.print("<font style='color:red;'>请先登录!</font>");
            request.getRequestDispatcher("login.html").include(request, response);
        }
        out.close();
    }
}
~~~