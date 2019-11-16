# 取值

比如使用JSTL输出要写成
~~~jsp
<c:out value="${name}" /> 
~~~
但是用EL只需要
`${name}`

# 作用域优先级

EL表达式可以从`pageContext`,`reques`t,`session`,`application`四个作用域中取到值，如果4个作用域都有`name`属性怎么办？

EL会按照从高到低的优先级顺序获取
`pageContext>request>session>application`

# 获取JavaBean的属性
获取JavaBean的属性，只需要通过.符号操作就可以了。

像这样` ${hero.name}` ，就会自动调用`getName`方法了
~~~jsp
<%
    Hero hero =new Hero();
    hero.setName("盖伦");
    hero.setHp(616);
     
    request.setAttribute("hero", hero);
%>
  
英雄名字 ： ${hero.name} <br>
英雄血量 ： ${hero.hp}
~~~

# 取参
EL表达式还可以做到`request.getParameter("name") `这样的形式获取浏览器传递过来的参数
先把jstl.jsp代码改为如例所示，然后访问如下地址
~~~shell
http://127.0.0.1/jstl.jsp?name=abc
~~~
可以观察到获取了参数 `name`

~~~jsp
${param.name}
~~~

# 条件判断

1. eq相等 ne、neq不相等，
2. gt大于， lt小于
3. gte、ge大于等于
4. lte、le 小于等于
5. not非 mod求模
6. is [not] div by是否能被某数整除
7. is [not] even是否为偶数
8. is [not] odd是否为奇

~~~jsp
<%
request.setAttribute("killNumber", "10");
%>
 
c:if 的用法，运行结果：
<c:if test="${killNumber>=10}">
超神
</c:if>
<c:if test="${killNumber<10}">
还没超神
</c:if>
<br>
c:choose 的用法，运行结果：
 
<c:choose>
    <c:when test="${killNumber>=10}">
        超神
    </c:when>
    <c:otherwise>
        还没超神
    </c:otherwise>
</c:choose>
<br>
EL表达式eq的用法，运行结果：
${killNumber ge 10? "超神":"还没超神" }
~~~