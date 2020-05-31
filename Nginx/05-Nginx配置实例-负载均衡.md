- [1. **首先准备两个同时启动的** **Tomcat**](#1-首先准备两个同时启动的-tomcat)
- [2. **在 nginx.conf 中进行配置**](#2-在-nginxconf-中进行配置)

# 1. **首先准备两个同时启动的** **Tomcat**

# 2. **在 nginx.conf 中进行配置**

**在 nginx.conf 中进行配置**

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521183833.png)

随着互联网信息的爆炸性增长，负载均衡（load balance）已经不再是一个很陌生的话题，顾名思义，负载均衡即是将负载分摊到不同的服务单元，既保证服务的可用性，又保证响应足够快，给用户很好的体验。快速增长的访问量和数据流量催生了各式各样的负载均衡产品，很多专业的负载均衡硬件提供了很好的功能，但却价格不菲，这使得负载均衡软件大受欢迎， nginx 就是其中的一个，在 linux 下有 Nginx、LVS、Haproxy 等等服务可以提供负载均衡服务，而且 Nginx 提供了几种分配方式(策略)：

 **1、轮询(默认)**

每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器 down 掉，能自动剔除。

**2、weight**

weight 代表权,重默认为 1,权重越高被分配的客户端越多

 指定轮询几率，weight 和访问比率成正比，用于后端服务器性能不均的情况。 例如：

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521184047.png)

**3、ip_hash**

每个请求按访问 ip 的 hash 结果分配，这样每个访客固定访问一个后端服务器，可以解决 session 的问题。例如：

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521184132.png)

**4、fair（第三方）**

按后端服务器的响应时间来分配请求，响应时间短的优先分配。

 ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521184224.png)