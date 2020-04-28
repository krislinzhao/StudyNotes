# 增加class
通过addClass() 增加一个样式中的class
~~~javascript
<script>
$(function(){
   $("#b1").click(function(){
      $("#d").addClass("pink");
   });
  
});
  
</script>
  <button id="b1">增加背景色</button>
<br>
<br>
 
<style>
.pink{
   background-color:pink;
}
</style>
  
<div id="d">
  
Hello JQuery
  
</div>
~~~

# 删除class
通过removeClass() 删除一个样式中的class
~~~javascript
script>
$(function(){
   $("#b1").click(function(){
      $("#d").removeClass("pink");
   });
  
});
  
</script>
  <button id="b1">删除背景色</button>
<br>
<br>
 
<style>
.pink{
   background-color:pink;
}
</style>
  
<div id="d" class="pink">
  
Hello JQuery
  
</div>
~~~

# 切换class
通过toggleClass() 切换一个样式中的class
这里的切换，指得是：
如果存在就删除
如果不存在，就添加
~~~javascript
<script>
$(function(){
   $("#b1").click(function(){
      $("#d").toggleClass("pink");
   });
   
});
   
</script>
  <button id="b1">切换背景色</button>
<br>
<br>
  
<style>
.pink{
   background-color:pink;
}
</style>
   
<div id="d" >
   
Hello JQuery
   
</div>
~~~

# css函数
通过css函数 直接设置样式
`css(property,value)`
第一个参数是样式属性，第二个参数是样式值
`css({p1:v1,p2:v2})`

参数是 {} 包含的属性值对。
属性值对之间用 逗号，分割
属性值之间用 冒号 ：分割
属性和值都需要用引号 “

~~~javascript
<script>
$(function(){
   $("#b1").click(function(){
      $("#d1").css("background-color","pink");
   });
   
   $("#b2").click(function(){
      $("#d2").css({"background-color":"pink","color":"green"});
   });
 
});
   
</script>
  <button id="b1">设置单一样式</button>
  <button id="b2">设置多种样式</button>
<br>
<br>
  
<div id="d1" >
   
单一样式，只设置背景色
   
</div>
 
<div id="d2" >
   
多种样式，不仅设置背景色，还设置字体颜色
   
</div>
~~~