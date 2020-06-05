# 元素 $("tagName")
根据 标签名 选择所有该标签的元素
~~~javascript
<script>
$(function(){
   $("#b1").click(function(){
      $("div").addClass("pink");
   });
   
});
   
</script>
  <button id="b1">给所有的div元素增加背景色</button>
<br>
<br>
  
<style>
.pink{
   background-color:pink;
}
</style>
   
<div >
Hello JQuery
</div>
 
<div >
Hello JQuery
</div>
 
<div >
Hello JQuery
</div>
~~~

# id    $("#id")
根据 id 选择元素
id应该是唯一的，如果id重复，则只会选择第一个。
~~~javascript
<script>
$(function(){
   $("#b1").click(function(){
      $("#d1").addClass("pink");
   });
    
});
    
</script>
  <button id="b1">给id=d1的div增加背景色</button>
<br>
<br>
   
<style>
.pink{
   background-color:pink;
}
</style>
    
<div id="d1">
Hello JQuery
</div>
  
<div id="d2" >
Hello JQuery
</div>
  
<div  id="d3">
Hello JQuery
</div>
~~~

# 类   $(".className")
根据 class 选择元素
~~~javascript
<script>
$(function(){
   $("#b1").click(function(){
      $(".d").addClass("pink");
   });
    
});
    
</script>
  <button id="b1">给class='d'的div增加背景色</button>
<br>
<br>
   
<style>
.pink{
   background-color:pink;
}
</style>
    
<div class="d">
Hello JQuery
</div>
  
<div class="d" >
Hello JQuery
</div>
  
<div  >
Hello JQuery
</div>
~~~

# 层级  $("selector1 selector2")
选择 selector1下的selector2元素 。
在本例中 选择id=d3的div下的span元素
~~~javascript
<script>
$(function(){
   $("#b1").click(function(){
      $("div#d3 span").addClass("pink");
   });
     
});
     
</script>
  <button id="b1">给id='d3'的div 下的 span 增加背景色</button>
<br>
<br>
    
<style>
.pink{
   background-color:pink;
}
</style>
     
<div class="d">
  <span>Hello JQuery</span>
    
</div>
   
<div class="d" >
  <span>Hello JQuery</span>
</div>
   
<div id="d3" >
  <span>Hello JQuery</span>
</div>
~~~

# 最先最后
`$(selector:first) `满足选择器条件的第一个元素
`$(selector:last) `满足选择器条件的最后一个元素

~~~javascript
<script>
$(function(){
   $("#b1").click(function(){
      $("div:first").addClass("pink");
   });
      
   $("#b2").click(function(){
      $("div:last").addClass("pink");
   });
 
});
      
</script>
  <button id="b1">第一个增加背景色</button>
  <button id="b2">最后一个增加背景色</button>
<br>
<br>
     
<style>
.pink{
   background-color:pink;
}
</style>
      
<div>
  <span>Hello JQuery</span>
     
</div>
    
<div >
  <span>Hello JQuery</span>
</div>
    
<div >
  <span>Hello JQuery</span>
</div>
~~~~

# 奇偶
$(selector:odd) 满足选择器条件的奇数元素
$(selector:even) 满足选择器条件的偶数元素
因为是基零的，所以第一排的下标其实是0(是偶数)

~~~javascript
<script>
$(function(){
   $("#b1").click(function(){
      $("div:odd").toggleClass("pink");
   });
       
   $("#b2").click(function(){
      $("div:even").toggleClass("green");
   });
  
});
       
</script>
  <button id="b1">切换奇数背景色</button>
  <button id="b2">切换偶数背景色</button>
<br>
<br>
      
<style>
.pink{
   background-color:pink;
}
.green{
   background-color:green;
}
</style>
       
<div>
  <span>Hello JQuery 0</span>
      
</div>
 
<div>
  <span>Hello JQuery 1</span>
      
</div>
     
<div >
  <span>Hello JQuery 2</span>
</div>
     
<div >
  <span>Hello JQuery 3</span>
</div>
 
<div >
  <span>Hello JQuery 4</span>
</div>
 
</div>
     
<div >
  <span>Hello JQuery 5</span>
</div>
     
<div >
  <span>Hello JQuery 6</span>
</div>
~~~

# 可见性
`$(selector:hidden)` 满足选择器条件的不可见的元素
`$(selector:visible) `满足选择器条件的可见的元素
注； `div:visible` 和`div :visible`(有空格)是不同的意思
`div:visible` 表示选中可见的div
`div :visible(有空格) `表示选中div下可见的元素
~~~javascript
<script>
$(function(){
   $("#b1").click(function(){
     $("div:visible").hide();
   });
   $("#b2").click(function(){
      $("div:hidden").show();     
   });
});
         
</script>
  <button id="b1">隐藏可见的</button>
  <button id="b2">显示不可见的</button>
  
<br>
<br>
        
<style>
.pink{
   background-color:pink;
}
  
</style>
         
<div>
  <span>Hello JQuery 1</span>
        
</div>
       
<div >
  <span>Hello JQuery 2</span>
</div>
       
<div >
  <span>Hello JQuery 3</span>
</div>
   
<div >
  <span >Hello JQuery 4</span>
</div>
   
</div>
       
<div >
  <span>Hello JQuery 5</span>
</div>
       
<div >
  <span>Hello JQuery 6</span>
</div>
~~~

# 属性
`$(selector[attribute]) `满足选择器条件的有某属性的元素
`$(selector[attribute=value]) `满足选择器条件的属性等于value的元素
`$(selector[attribute!=value])` 满足选择器条件的属性不等于value的元素
`$(selector[attribute^=value])` 满足选择器条件的属性以value开头的元素
`$(selector[attribute$=value]) `满足选择器条件的属性以value结尾的元素
`$(selector[attribute*=value])` 满足选择器条件的属性包含value的元素


注： 一般不要使用`[class=className] `而应该使用`.className`
举出不完整的部分代码示意
~~~javascript
<style>
.border{
border :1px solid blue
}
</style>
<div class = "class1">class = class1的div</div>
<script>
$("#b7").click(function(){
               $("div[class = class1]").toggleClass("border");

           });
</script>
~~~
一开始使用`toggleClass("border")`;  会因为不存在属性`border`而添加   即`<div class = "class1">`变成了`<div class = "class1 border">`   因此`div[class = "class1]`已经不存在了，无法再被选择，如果一定要选择只能是去选择`div[class = class1 border]`



~~~javascript
<script>
$(function(){
   $("#b1").click(function(){
      $("div[id]").toggleClass("border");
   });
   $("#b2").click(function(){
      $("div[id='pink']").toggleClass("border");
   });
   $("#b3").click(function(){
      $("div[id!='pink']").toggleClass("border");
   });
   $("#b4").click(function(){
      $("div[id^='p']").toggleClass("border");
   });
   $("#b5").click(function(){
      $("div[id$='k']").toggleClass("border");
   });
   $("#b6").click(function(){
      $("div[id*='ee']").toggleClass("border");
   });
 
});
        
</script>
  <button id="b1">给有id属性的div切换边框</button>
  <button id="b2">给id=pink的div切换边框</button>
  <button id="b3">给有id!=pink属性的div切换边框</button>
  <button id="b4">给有id以p开头的div切换边框</button>
  <button id="b5">给有id以k结尾的div切换边框</button>
  <button id="b6">给有id包含ee的div切换边框</button>
 
<br>
<br>
       
<style>
.pink{
   background-color:pink;
}
.green{
   background-color:green;
}
.border{
   border: 1px blue solid;
    
}
 
button{
   margin-top:10px;
   display:block;
}
 
div{
  margin:10px;
}
</style>
        
<div id="pink">
    id=pink的div
</div>
      
<div id="green">
  id=green的div
</div>
      
<div >
   没有id的div
</div>
~~~

# 表单对象
表单对象选择器 指的是选中form下会出现的输入元素
`:input` 会选择所有的输入元素，不仅仅是`input`标签开始的那些，还包括`textarea`,`select`和`button`
`:button `会选择`type=button`的`input`元素和`button`元素
`:radio `会选择单选框
`:checkbox`会选择复选框
`:text`会选择文本框，但是不会选择文本域
`:submit`会选择提交按钮
`:image`会选择图片型提交按钮
`:reset`会选择重置按钮

注意: 
`$("td[rowspan!=13] "+value).toggle(500);`
`$("td[rowspan!=13] `后面有一个空格，表示层级选择器，如果没有就会出错
toggle(500) 表示在显示与隐藏之间来回切换，生效时间是500毫秒

注：`:submit`会把`<button>`元素选中，因为在一些浏览器中，`<button>`元素的type默认值是`submit`，所以会选中它。

~~~javascript
<script>
$(function(){
   $(".b").click(function(){
      var value = $(this).val();
      $("td[rowspan!=13] "+value).toggle(500);
   });
        
});
        
</script>
 
<style>
table{
    border-collapse:collapse;
        table-layout:fixed;
    width:80%;
}
table td{
    border-bottom: 1px solid #ddd;
    padding-bottom: 5px;
    padding-top: 5px;
}
 
div button{
    display:block;
}
 
</style>
 
<table>
    <tr>
         
        <td rowspan="13" valign="top" width="150px">
            <div >
                <button value=":input" class="b">切换所有的:input</button>
                <button value=":button" class="b">切换:button</button>
                <button value=":radio" class="b">切换:radio</button>     
                <button value=":checkbox" class="b">切换:checkbox</button>       
                <button value=":text" class="b">切换:text</button>       
                <button value=":password" class="b">切换:password</button>       
                <button value=":file" class="b">切换:file</button>       
                <button value=":submit" class="b">切换:submit</button>       
                <button value=":image" class="b">切换:image</button>     
                <button value=":reset" class="b">切换:reset</button>         
            </div>           
        </td>
        <td width="120px">说明</td>
        <td width="120px">表单对象</td>
        <td width="">示例</td>
    </tr>
<tr>
  <td >input按钮</td>
  <td >:button</td>
  <td><input type="button" value="input按钮"/></td>
</tr>
 
<tr>
  <td>button按钮</td>
  <td >:button</td>
  <td><button>Button按钮</button></td>
</tr>
<tr>
  <td>单选框</td>
  <td >:radio</td>
  <td><input type="radio" ></td>
</tr>
<tr>
  <td>复选框</td>
  <td >:checkbox</td>
  <td><input type="checkbox"  ></td>
</tr>
 
<tr>
  <td>文本框</td>
  <td >:text</td>
  <td><input type="text" /></td>
</tr>
<tr>
  <td>文本域</td>
  <td ></td>
  <td><textarea></textarea></td>
</tr>
<tr>
  <td>密码框</td>
  <td >:password</td>
  <td><input type="password" /></td>
</tr>
<tr>
  <td>下拉框</td>
  <td ></td>
  <td><select><option>Option</option></select></td>
</tr>
 
<tr>
  <td>文件上传</td>
  <td >:file</td>
  <td> <input type="file" /></td>
</tr>
 
<tr>
  <td>提交按钮</td>
  <td >:submit</td>
  <td><input type="submit" /></td>
</tr>
<tr>
  <td>图片型提交按钮</td>
  <td >:image</td>
  <td><input type="image" src="https://how2j.cn/example.gif" /></td>
</tr>
 
<tr>
  <td>重置按钮</td>
  <td >:reset</td>
  <td><input type="reset" /></td>
</tr>
 
</table>
~~~

# 表单对象属性
`:enabled`会选择可用的输入元素 注：输入元素的默认状态都是可用
`:disabled`会选择不可用的输入元素
`:checked`会选择被选中的单选框和复选框 注： checked在部分浏览器上(火狐,chrome)也可以选中selected的option
`:selected`会选择被选中的option元素

~~~javascript
<script>
$(function(){
   $(".b").click(function(){
      var value = $(this).val();
      $("td[rowspan!=13] "+value).toggle(500);
   });
    
   $(".b2").click(function(){
      var value = $(this).val();
      var options = $("td[rowspan!=13] "+value);
      alert("选中了"+options.length+"条记录！");
   });
        
});
        
</script>
 
<style>
table{
    border-collapse:collapse;
        table-layout:fixed;
    width:80%;
}
table td{
    border-bottom: 1px solid #ddd;
    padding-bottom: 5px;
    padding-top: 5px;
}
 
div button{
    display:block;
}
 
.border{
   border: 1px blue solid;
     
}
 
</style>
 
<table>
    <tr>
         
        <td rowspan="13" valign="top" width="120">
            <div >
                <button value=":enabled" class="b">切换:enabled</button>
                <button value=":disabled" class="b">切换:disabled</button>       
                <button value=":checked" class="b">切换:checked</button>     
                <button value=":selected" class="b2">:selected数量</button>      
            </div>           
        </td>
        <td width="120">说明</td>
        <td width="120">表单对象属性</td>
        <td width="100px">示例</td>
    </tr>
<tr>
  <td >enabled的按钮</td>
  <td >:enabled</td>
  <td><input type="button" enabled="enabled" value="enabled的按钮"/></td>
</tr>
 
<tr>
  <td >disabled的按钮</td>
  <td >:disabled</td>
  <td><input type="button" disabled="disabled" value="disabled的按钮"/></td>
</tr>
 
  <td >选中的复选框</td>
  <td >:checked</td>
  <td>
     
    <input type="radio" checked="checked" ><br>
    <input type="radio" ><br>
    <input type="checkbox" ><br>
    <input type="checkbox" checked="checked" >
  </td>
 
<tr>
  <td>选中的下拉列表</td>
  <td >:selected</td>
  <td>
    <select size="3" multiple="multiple">
       <option selected="selected">苍老师</option>
       <option >高树玛利亚</option>
       <option selected="selected">遥美</option>
    </select>
  </td>
</table>
 
<form>
 
</form>
~~~

# 当前元素
在监听函数中使用 $(this)，即表示触发该事件的组件。
~~~javascript
<script>
$(function(){
   $("#b1").click(function(){
     $(this).hide();
   });
    
});
</script>
  
<button id="b1">点击隐藏</button>
~~~
