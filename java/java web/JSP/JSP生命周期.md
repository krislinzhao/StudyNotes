# JSP执行过程
以下是JSP遵循的过程 
* 编译
* 初始化
* 执行
* 清理

JSP生命周期的四个主要阶段与Servlet生命周期非常相似。以下描述了四个阶段 

# 1.JSP编译
当浏览器请求JSP时，JSP引擎首先检查是否需要编译页面。如果页面从未被编译，或者JSP从上一次编译以来JSP文件代码已被修改，那么JSP引擎将会编译页面。
编译过程包括三个步骤 -
* 解析JSP。
* 将JSP转换为servlet。
* 编译servlet。

# 2.JSP初始化
当容器加载JSP时，它会在处理任何请求之前调用`jspInit()`方法。 如果需要执行特定于JSP的初始化，那么可以覆盖`jspInit()`方法 -
~~~java
public void jspInit(){
   // Initialization code...
}
~~~
通常，初始化仅执行一次，并且与servlet的`init()`方法一样，一般会在`jspInit()`方法中初始化数据库连接，打开文件和创建查找表。

# 3.JSP执行
JSP生命周期的这个阶段表示所有与请求的交互，直到JSP被销毁为止。
每当浏览器请求JSP并且页面已被加载和初始化时，JSP引擎将调用JSP中的`_jspService()`方法。
`_jspService()`方法以`HttpServletRequest`和`HttpServletResponse`为参数，如下所示：
~~~java
void _jspService(HttpServletRequest request, HttpServletResponse response) {
   // Service handling code...
}
~~~
根据请求调用JSP的`_jspService()`方法。它负责生成请求的响应，此方法还负责生成对所有七种HTTP方法的响应，即`GET`，`POST`，`DELETE`等。

# 4.JSP清理
JSP生命周期的清理阶段表示当JSP被容器从使用中移除时。
`jspDestroy()`方法是等效于servlet的`destroy`方法的JSP方法。当需要执行清理工作时，可以覆盖`jspDestroy()`方法，如：释放数据库连接或关闭打开的文件。
`jspDestroy()`方法具有以下形式 
~~~java
public void jspDestroy() {
   // Your cleanup code goes here.
}
~~~
