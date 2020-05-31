- [1. 什么是动静分离](#1-什么是动静分离)
- [2. 准备工作](#2-准备工作)
- [3. 具体配置](#3-具体配置)
- [4. 最终测试](#4-最终测试)

# 1. 什么是动静分离

Nginx 动静分离简单来说就是把动态跟静态请求分开，不能理解成只是单纯的把动态页面和静态页面物理分离。严格意义上说应该是动态请求跟静态请求分开，可以理解成使用 Nginx 处理静态页面，Tomcat 处理动态页面。动静分离从目前实现角度来讲大致分为两种，

**一种是纯粹把静态文件独立成单独的域名，放在独立的服务器上，也是目前主流推崇的方案**；

**另外一种方法就是动态跟静态文件混合在一起发布，通过 nginx 来分开**。

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521204548.png)

通过 location 指定不同的后缀名实现不同的请求转发。通过 expires 参数设置，可以使浏览器缓存过期时间，减少与服务器之前的请求和流量。具体 Expires 定义：是给一个资源设定一个过期时间，也就是说无需去服务端验证，直接通过浏览器自身确认是否过期即可，所以不会产生额外的流量。此种方法非常适合不经常变动的资源。（如果经常更新的文件，不建议使用 Expires 来缓存），我这里设置 3d，表示在这 3 天之内访问这个 URL，发送一个请求，比对服务器该文件最后更新时间没有变化，则不会从服务器抓取，返回状态码304，如果有修改，则直接从服务器重新下载，返回状态码 200。

# 2. 准备工作

**在 liunx 系统中准备静态资源，用于进行访问**

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521204736.png)

# 3. 具体配置

**在** **nginx** **配置文件中进行配置**

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521204831.png)

# 4. 最终测试

**浏览器中输入地址** 

http://服务器ip/image/01.jpg

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521205251.png)

**因为配置文件** **autoindex on**

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521205456.png)

**在浏览器地址栏输入地址**

http://服务器ip/www/a.html

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521205633.png)