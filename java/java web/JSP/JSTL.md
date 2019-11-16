在页面中使用JSTL需要在jsp中 通过指令进行设置
~~~jap
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
~~~
`prefix="c"` 表示后续的标签使用都会以`<c:` 开头

~~~jsp
<c:set var="name" value="${'gareen'}" scope="request" />

通过标签获取name: <c:out value="${name}" /> <br>

<c:remove var="name" scope="request" /> <br>

通过标签获取name: <c:out value="${name}" /> <br>
~~~

# 判断语句 
JSTL通过`<c:if test="">` 进行条件判断

但是JSTL没有`<c:else`，所以常用的办法是在`<c:if`的条件里取反

配合`if`使用的还有通过`empty`进行为空判断
`empty`可以判断对象是否为`null`,字符串长度是否为`0`，集合长度是否为`0`
~~~jsp
<c:set var="hp" value="${10}" scope="request"/>

<c:if test="${hp<5}">
    <p>这个英雄要挂了</p>
</c:if>

<c:if test="${!(hp<5)}">
    <p>这个英雄觉得自己还有救</p>
</c:if>

<c:set var="weapon" value="${null}" scope="page"/>
<c:set var="lastwords" value="${''}" scope="page"/>
<%
    List lis = new ArrayList();
%>
<c:set var="items" value="${list}" scope="page"/>

<c:if test="${empty weapon}">
    <p>没有武器了</p>
</c:if>
<c:if test="${empty lastwords}">
    <p>挂了也没有遗言</p>
</c:if>
<c:if test="${empty items}">
    <p>物品栏为空</p>
</c:if>
~~~

**choose语句**
~~~jsp
<c:choose>
    <c:when test="${hp1<5}">
        <p>这个英雄觉得自己要挂了</p>
    </c:when>
    <c:otherwise>
        <p>这个英雄觉得自己还可以再抢救抢救</p>
    </c:otherwise>
</c:choose>
~~~

# forEach
`items="${heros}" `表示遍历的集合
`var="hero" `表示把每一个集合中的元素放在hero上
`varStatus="st" `表示遍历的状态
~~~jsp
<%
    List<String> heros = new ArrayList<String>();
    heros.add("塔姆");
    heros.add("艾克");
    heros.add("巴德");
    heros.add("雷克赛");
    heros.add("卡莉丝塔");
    request.setAttribute("heros",heros);
%>
<table width="200px" align="center" border="1" cellspacing="0">
    <tr>
        <td>编号</td>
        <td>英雄</td>
    </tr>
    <c:forEach var="hero" items="${heros}" varStatus="st">
        <tr>
            <td><c:out value="${st.count}"/> </td>
            <td><c:out value="${hero}"/> </td>
        </tr>
    </c:forEach>
</table>
~~~

# forToken
`<c:forTokens`专门用于字符串拆分，并且可以指定多个分隔符
~~~jsp
<c:set var="heros" value="塔姆,艾克;巴德|雷克赛!卡莉丝塔" />
<c:forTokens items="${heros}" delims=":;|;!" var="hero">
    <c:out value="${hero}"/><br/>
</c:forTokens>
~~~

# fmt:formatNumber 格式化数字
fmt 标签常用来进行格式化，其中fmt:formatNumber用于格式化数字
使用之前要加上
~~~jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt"  prefix='fmt' %>  
~~~

~~~jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt"  prefix='fmt' %>
<c:set var="moeny" value="888.8888"/>
<c:set var="pi" value="3.1415926"/>
<fmt:formatNumber type="number" value="${moeny}" minFractionDigits="2"/><br/>
<fmt:formatNumber type="number" value="${pi}" maxFractionDigits="2"/><br/>
~~~
`<fmt:formatNumber `表示格式化数字
`minFractionDigits `小数点至少要有的位数
`maxFractionDigits `小数点最多能有的位数

# fmt:formatDate 格式化日期

`<fmt:formatDate `表示格式化日期
`yyyy` 表示年份
`MM` 表示月份
`dd` 表示日期
`E `表示星期几

`a` 表示是上午还是下午
`HH `表示小时
`mm` 表示分钟
`ss` 表示秒
`S` 表示毫秒
`z` 表示时区

~~~jsp
<%
    Date now = new Date();
    pageContext.setAttribute("now",now);
%>
 
完整日期: <fmt:formatDate value="${now}" pattern="G yyyy年MM月dd日 E"/><br>
完整时间: <fmt:formatDate value="${now}" pattern="a HH:mm:ss.S z"/><br>
常见格式: <fmt:formatDate value="${now}" pattern="yyyy-MM-dd HH:mm:ss"/>
~~~

# fn:
fn标签提供各种实用功能，首先使用之前使用加入如下指令
~~~jsp
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %> 
~~~
用法举例：
`${fn:substring(name, 0, 5)}`

函数|描述
--|--
`fn:contains(string, substring)`|如果参数string中包含参数substring，返回true
`fn:containsIgnoreCase(string, substring)`|如果参数string中包含参数substring（忽略大小写），返回true
`fn:endsWith(string, suffix)`|如果参数 string 以参数suffix结尾，返回true
`fn:escapeXml(string)`|将有特殊意义的XML (和HTML)转换为对应的XML character entity code，并返回
`fn:indexOf(string, substring)`|返回参数substring在参数string中第一次出现的位置
`fn:join(array, separator)`|将一个给定的数组array用给定的间隔符separator串在一起，组成一个新的字符串并返回。
`fn:length(item)`|返回参数item中包含元素的数量。参数Item类型是数组、collection或者String。如果是String类型,返回值是String中的字符数。
`fn:replace(string, before, after)`|返回一个String对象。用参数after字符串替换参数string中所有出现参数before字符串的地方，并返回替换后的结果
`fn:split(string, separator)`|返回一个数组，以参数separator 为分割符分割参数string，分割后的每一部分就是数组的一个元素
`fn:startsWith(string, prefix)`|如果参数string以参数prefix开头，返回true
`fn:substring(string, begin, end)`|返回参数string部分字符串, 从参数begin开始到参数end位置，包括end位置的字符
`fn:substringAfter(string, substring)`|返回参数substring在参数string中后面的那一部分字符串
`fn:substringBefore(string, substring)`|返回参数substring在参数string中前面的那一部分字符串
`fn:toLowerCase(string)`|将参数string所有的字符变为小写，并将其返回
`fn:toUpperCase(string)`|将参数string所有的字符变为大写，并将其返回
`fn:trim(string)`|去除参数string 首尾的空格，并将其返回
