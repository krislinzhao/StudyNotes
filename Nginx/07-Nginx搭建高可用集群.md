- [1. Keepalived+Nginx 高可用集群（主从模式）](#1-keepalivednginx-高可用集群主从模式)
- [2. 配置高可用的准备工作](#2-配置高可用的准备工作)
- [3. 在两台服务器上安装keepalived](#3-在两台服务器上安装keepalived)
- [4. 完成高可用配置(主从配置)](#4-完成高可用配置主从配置)
- [5. 最终测试](#5-最终测试)

# 1. Keepalived+Nginx 高可用集群（主从模式）

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521211609.png)

# 2. 配置高可用的准备工作

1. 需要两台服务器
2. 需要keepalived
3. 需要虚拟ip

# 3. 在两台服务器上安装keepalived

1. 使用yum命令安装

   ```shell
   yum install keepalived-v
   ```

2. 安装完成之后，在etc里面生成keepalived，有文件keepalived.conf

# 4. 完成高可用配置(主从配置)

1. 修改`/etc/keepalived/keepalived.conf`配置文件

```properties
global_defs {

notification_email {

acassen@firewall.loc

failover@firewall.loc

sysadmin@firewall.loc

}

notification_email_from Alexandre.Cassen@firewall.loc

smtp_server 192.168.17.129

smtp_connect_timeout 30

router_id LVS_DEVEL

}

vrrp_script chk_http_port {
script "/usr/local/src/nginx_check.sh"

interval 2	#（检测脚本执行的间隔）

weight 2

}

vrrp_instance VI_1 {

state BACKUP	# 备份服务器上将 MASTER 改为 BACKUP

interface ens33	//网卡

virtual_router_id 51	# 主、备机的 virtual_router_id 必须相同

priority 100	# 主、备机取不同的优先级，主机值较大，备份机值较小

advert_int 1

authentication {

auth_type PASS


auth_pass 1111

}

virtual_ipaddress {

192.168.17.50 // VRRP H 虚拟地址

}

}

```

2. 在`/usr/local/src`添加检测脚本

```bash
#!/bin/bash
A=`ps -C nginx –no-header |wc -l`
if [ $A -eq 0 ];
    then /usr/local/nginx/sbin/nginx
    sleep 2
    if [ `ps -C nginx --no-header |wc -l` -eq 0 ];
    	then killall keepalived
    fi
 fi
```

3. 把两台服务器上nginx和keepalived启动

启动nginx  

```shell
systemctl start nginx
```

启动keepalived

```shell
systemctl start keepalived.service
```

# 5. 最终测试

1. 在浏览器地址栏输入虚拟地址ip 192.168.17.50

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521213142.png)

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521213213.png)

2. 把主服务器(192.168.17.129) nginx和keepalived停止，在输入192.168.17.50

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521213341.png)

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200521213419.png)