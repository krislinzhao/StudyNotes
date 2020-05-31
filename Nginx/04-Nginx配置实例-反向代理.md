- [1. 反向代理实例一](#1-反向代理实例一)
  - [实现过程](#实现过程)
    - [1. 启动一个 tomcat，浏览器地址栏输入 127.0.0.1:8080，出现如下界面](#1-启动一个-tomcat浏览器地址栏输入-1270018080出现如下界面)
    - [2. 通过修改本地 host 文件，将 www.123.com 映射到 127.0.0.1](#2-通过修改本地-host-文件将-www123com-映射到-127001)
    - [3. **在 nginx.conf 配置文件中增加如下配置**](#3-在-nginxconf-配置文件中增加如下配置)
- [2. 反向代理实例二](#2-反向代理实例二)
  - [实现过程](#实现过程-1)
    - [1.准备两个 tomcat，一个 8001 端口，一个 8002 端口，并准备好测试的页面](#1准备两个-tomcat一个-8001-端口一个-8002-端口并准备好测试的页面)
    - [2. 修改 nginx 的配置文件在 http 块中添加 server{}](#2-修改-nginx-的配置文件在-http-块中添加-server)

# 1. 反向代理实例一

实现效果：使用 nginx 反向代理，访问 www.123.com 直接跳转到 127.0.0.1:8080

## 实现过程

### 1. 启动一个 tomcat，浏览器地址栏输入 127.0.0.1:8080，出现如下界面

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521173446.png)

### 2. 通过修改本地 host 文件，将 www.123.com 映射到 127.0.0.1

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521173537.png)

配置完成之后，我们便可以通过 www.123.com:8080 访问到第一步出现的 Tomcat 初始界面。那么如何只需要输入 www.123.com 便可以跳转到 Tomcat 初始界面呢？便用到 nginx 的反向代理。

### 3. **在 nginx.conf 配置文件中增加如下配置**

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521173630.png)

如上配置，我们监听 80 端口，访问域名为 www.123.com，不加端口号时默认为 80 端口，故访问该域名时会跳转到 127.0.0.1:8080 路径上。在浏览器端输入 www.123.com 结果如下：

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521173703.png)

# 2. 反向代理实例二

实现效果：使用 nginx 反向代理，根据访问的路径跳转到不同端口的服务中 nginx 监听端口为 9001，

> 访问 http://127.0.0.1:9001/edu/ 直接跳转到 127.0.0.1:8081 
>
> 访问 http://127.0.0.1:9001/vod/ 直接跳转到 127.0.0.1:8082

## 实现过程

### 1.准备两个 tomcat，一个 8001 端口，一个 8002 端口，并准备好测试的页面

### 2. 修改 nginx 的配置文件在 http 块中添加 server{}

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521173939.png)

**location** **指令说明**   该指令用于匹配 URL。

语法如下：

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521174037.png)

1. = ：用于不含正则表达式的 uri 前，要求请求字符串与 uri 严格匹配，如果匹配成功，就停止继续向下搜索并立即处理该请求。
2. ~：用于表示 uri 包含正则表达式，并且区分大小写。
3. ~*：用于表示 uri 包含正则表达式，并且不区分大小写。
4. ^~：用于不含正则表达式的 uri 前，要求 Nginx 服务器找到标识 uri 和请求字符串匹配度最高的 location 后，立即使用此 location 处理请求，而不再使用 location 块中的正则 uri 和请求字符串做匹配。

**注意：如果 uri 包含正则表达式，则必须要有 ~ 或者 ~* 标识。**

 