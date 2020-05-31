- [场景](#场景)
- [初步解决](#初步解决)
  - [1. 进入容器的tomcat目录](#1-进入容器的tomcat目录)
  - [2. 进入webapps文件夹](#2-进入webapps文件夹)
  - [3.把webapps.dist中的内容复制到webapps中](#3把webappsdist中的内容复制到webapps中)
- [彻底解决](#彻底解决)
  - [1. ctrl+p+q不退出容器的方式返回到宿主机目录下](#1-ctrlpq不退出容器的方式返回到宿主机目录下)
  - [2. 使用docker commit命令将修改后的容器生成新的镜像](#2-使用docker-commit命令将修改后的容器生成新的镜像)
  - [3.运行新的镜像](#3运行新的镜像)

# 场景

docker启动tomcat(版本是从阿里云上拉下的:9.0.35)时,访问tomcat首页时出现404错误

# 初步解决

## 1. 进入容器的tomcat目录

使用命令: `docker exec -it 运行的tomcat容器ID /bin/bash `进入到tomcat的目录

![](https://cdn.jsdelivr.net/gh/krislinzhao/IMGcloud/img/20200514180618.png)

## 2. 进入webapps文件夹

发现里面是空的(tomcat默认的欢迎页面实际上放在的路径应该是:webapps/ROOT/index.jsp或者index.html)

## 3.把webapps.dist中的内容复制到webapps中

![](https://cdn.jsdelivr.net/gh/krislinzhao/IMGcloud/img//20200514180909.png)

# 彻底解决

虽然这样解决了问题但是不够彻底，因为再另启动一个Tomcat容器时又会回到之前的情况。

## 1. ctrl+p+q不退出容器的方式返回到宿主机目录下

## 2. 使用docker commit命令将修改后的容器生成新的镜像

 docker commit命令详解: 

  * 1.作用：将运行着的容器映射成新的镜像

  * 2.格式: docker commit -a='作者' -m='‘修改内容--随意写' 容器名称或者ID 新生成镜像的名称

  * 3.例子:  docker commit -a='krislin' -m='将修改后的容器映射成新的镜像' tomcat krislin/tomcat 

    

## 3.运行新的镜像

访问tomcat首页，发现不会再出现404错误，以后每次创建tomcat容器时，使用我们自己生成的镜像即可(它跟阿里云拉下来的进行并没什么差别，只是保存了我们之前对容器做的修改)

![](https://cdn.jsdelivr.net/gh/krislinzhao/IMGcloud/img/20200514182733.png)

