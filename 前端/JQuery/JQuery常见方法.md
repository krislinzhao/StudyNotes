# 取值
通过JQuery对象的`val()`方法获取值
相当于` document.getElementById("input1").value;`
~~~javascript
<script>
$(function(){
   $("#b1").click(function(){
      alert($("#input1").val());
   });
});
   
</script>
   
<button id="b1">取值</button>
    
<br>
<br>
   
<input type="text" id="input1" value="默认值">
~~~

# 获取元素内容,如果有子元素，保留标签
通过html() 获取元素内容,如果有子元素，保留标签
~~~javascript
<script>
$(function(){
   $("#b1").click(function(){
      alert($("#d1").html());
   });
});
  
</script>
  
<button id="b1">获取文本内容</button>
   
<br>
<br>
  
<div id="d1">
这是div的内容
<span>这是div里的span
</span>
</div>
~~~

# 获取元素内容,如果有子元素，不包含子元素标签
通过text() 获取元素内容,如果有子元素，不包含标签
~~~javascript
<script src="https://how2j.cn/study/jquery.min.js"></script>
   
<script>
$(function(){
   $("#b1").click(function(){
      alert($("#d1").text());
   });
});
   
</script>
   
<button id="b1">获取文本内容</button>
    
<br>
<br>
   
<div id="d1">
这是div的内容
<span>这是div里的span
</span>
</div>
~~~

