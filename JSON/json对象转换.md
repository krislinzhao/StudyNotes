# 字符串转为JSON对象
通过字符串拼接得到一个JSON结构的字符串，并不是一个JSON对象。 需要通过eval转换得到
转换的时候注意,eval 函数要以`"("`开头，`")"`结尾
或者使用JQuery的$.parseJSON转换函数
~~~javascript
<script>
 
var s1 = "{\"name\":\"盖伦\"";
var s2 = ",\"hp\":616}";
var s3 = s1+s2;
 
document.write("这是一个JSON格式的字符串:" + s3);
document.write("<br>");
var gareen = eval("("+s3+")");
 
document.write("这是一个JSON对象: " + gareen);
  
</script>
~~~

# JSON 对象转换为字符串
json 对象因为是一个javascript对象，所以如果直接打印的话，看不到里面的内容。
有时候要看看这个对象是不是我们期望的，所以需要通过 `JSON.stringify` 函数把它转换为 字符串
~~~javascript
<script>
var hero = {"name":"盖伦","hp":"616"};
document.write("这是一个json 对象："+ hero);
document.write("<br>");
var heroString = JSON.stringify(hero)
document.write("这是一个json 字符串："+ heroString );
</script>
~~~