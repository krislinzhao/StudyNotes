# 1. 什么是Ajax

Ajax（Asynchronous JavaScript and XML的缩写）是一种异步请求数据的web开发技术，对于改善用户的体验和页面性能很有帮助。简单地说，在不需要重新刷新页面的情况下，Ajax 通过异步请求加载后台数据，并在网页上呈现出来。常见运用场景有表单验证是否登入成功、百度搜索下拉框提示和快递单号查询等等

**Ajax目的：提高用户体验，较少网络数据的传输量**

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200605115538.png)

# 2. Ajax例子

“老师让班长叫小明过来解释下作业为什么没有交”

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521134128.png)

1. 客户端发起一个数据请求 (老师让班长去叫小明). 

2. 客户端继续页面工作 (老师继续改作业). 

3. XMLHttpRequest向服务器请求数据 (班长去叫小明去老师办公室). 

4. 服务器返回数据给XMLHttpRequest (班长领着小明前往老师办公室). 

5. XMLHttpRequest通知客户端(浏览器)接收数据 (班长告诉老师,小明来了). 

6. 客户端(浏览器)接收XMLHttpRequest返回的数据,渲染页面 (小明向老师解释为什么作业没交). 

# 3. Ajax的使用

## 1. **创建Ajax核心对象XMLHttpRequest**

```javascript
//Ajax例子
//浏览器加载时
window.onload = function () {
    var xhl = null;
    if (window.XMLHttpRequest){
        // 兼容 IE7+, Firefox, Chrome, Opera, Safari
        xhl = new  XMLHttpRequest();
    }
    else {
        // 兼容 IE6, IE5 
        xhl = new ActiveXObject("Microsoft.XMLHTTP");
    }
}
```

## 2. **向服务器发送请求**

```javascript
   语法: xhr.open(method,url,async);
    
    xhl.open("GET","url",true);
    xhl.send(str);//post请求时才使用字符串参数，否则不用带参数。
```

method：请求的类型；GET 或 POST）

url：文件在服务器上的位置 ）

async：true（异步）或 false（同步）

**注意：post请求一定要设置请求头的格式内容**

```javascript
xhr.open("POST","test.html",true);  
xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");  
xhr.send("fname=Henry&lname=Ford");  //post请求参数放在send里面，即请求体
```

## 3. **服务器响应处理（区分同步跟异步两种情况）**

**responseText: 获得字符串形式的响应数据。**

**responseXML: 获得XML 形式的响应数据。**

### **①同步处理**

```javascript
    1. xhr.open("GET","info.txt",false);  
    2. xhr.send();  
    3. document.getElementById("myDiv").innerHTML=xhr.responseText; //获取数据直接显示在页面上
```

### **②异步处理**

相对来说比较复杂，要在请求状态改变事件中处理。

```javascript
    1.    xhr.onreadystatechange=function()  { 
    2.    if (xhr.readyState==4 &&xhr.status==200)  { 
    3.         document.getElementById("myDiv").innerHTML=xhr.res ponseText;  
    4.      }
    5.    }  
```

**readyState:**

0-（未初始化）还没有调用send()方法

1-（载入）已调用send()方法，正在发送请求

2-（载入完成）send()方法执行完成，已经接收到全部响应内容

3-（交互）正在解析响应内容

4-（完成）响应内容解析完成，可以在客户端调用了

**status:**

| ---  | 类别                            | 描述               |
| ---- | ------------------------------- | ------------------ |
| 1xx  | informational                   | 接收的请求正在处理 |
| 2xx  | Success (成功状态码)            | 请求正常处理完毕   |
| 3xx  | Redirection (重定向状态码)      | 请求正常处理完毕   |
| 4xx  | Client Error (客户端错误状态码) | 请求正常处理完毕   |
| 5xx  | Server Error (服务器错误状态码) | 请求正常处理完毕   |

### **③GET和POST请求数据区别**

请求数据时的区别，详情见下面两张图：

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200605115628.png)

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521134958.png)

总而言之：

- GET请求的差数直接拼接在url上面
- POST请求的参数就不是放在url了，而是放在send里面，即请求体四、结束语