# AJAX请求
![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200605115127.png)

# 创建XHR
   创建XHR对象 XMLHttpRequest
    XHR对象是一个javascript对象，它可以在用户没有感觉的情况下，就像背后运行的一根小线程一般，悄悄的和服务器进行数据交互
    AJAX就是通过它做到无刷新效果的
~~~javascript
<script>
	var xmlhttp =new XMLHttpRequest();
	document.write(xmlhttp);
</script>
~~~

# 设置并发出请求
XHR对象的作用是和服务器进行交互，所以既会发消息给服务器，也能接受服务器返回的响应。
当服务器响应回来的时候，调用怎么处理呢？
通过` xmlhttp.onreadystatechange=checkResult `就可以指定用`checkResult` 函数进行处理。


# 设置并发出请求   
通过open函数设置背后的这个小线程，将要访问的页面url ，在本例中就是
/study/checkName.jsp
`xmlhttp.open("GET",url,true);`
通过send函数进行实际的访问
`xmlhttp.send(null);`
`null`表示没有参数，因为参数已经通过`GET` 方式，放在`url`里了。
只有在用`POST`，并且需要发送参数的时候，才会使用到`send`。
类似这样：
`xmlhttp.send("user="+username+"&password="+password)`

# 处理响应信息
在checkResult 函数中处理响应
~~~javascript
function checkResult(){
  if (xmlhttp.readyState==4 && xmlhttp.status==200)  
     document.getElementById('checkResult').innerHTML=xmlhttp.responseText;
}
~~~

`xmlhttp.readyState` 4 表示请求已完成
`xmlhttp.status` 200 表示响应成功
`xmlhttp.responseText`; 用于获取服务端传回来的文本
`document.getElementById('checkResult').innerHTML` 设置span的内容为服务端传递回来的文本

# 步骤
1. 创建XHR对象
2. 设置响应函数
3. 设置要访问的页面
4. 发出请求
5. 当服务端的响应返回，响应函数被调用。
6. 在响应函数中，判断响应是否成功，如果成功获取服务端返回的文本，并显示在span中。

~~~javascript
<span>输入账号 :</span>
<input id="name" name="name" onkeyup="check()" type="text"> 
<span id="checkResult"></span>
  
<script>
var xmlhttp;
function check(){
  var name = document.getElementById("name").value;
  var url = "https://how2j.cn/study/checkName.jsp?name="+name;
  
  xmlhttp =new XMLHttpRequest();
  xmlhttp.onreadystatechange=checkResult; //响应函数
  xmlhttp.open("GET",url,true);   //设置访问的页面
  xmlhttp.send(null);  //执行访问
}
  
function checkResult(){
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    document.getElementById('checkResult').innerHTML=xmlhttp.responseText;
   
}  
</script>
~~~