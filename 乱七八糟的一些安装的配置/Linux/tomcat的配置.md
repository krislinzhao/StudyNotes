# 1.下载Tomcat
[Tomcat官网](https://tomcat.apache.org/download-80.cgi)

# 2.解压到相应的目录
```bash
tar -xzvf 下载的linux版tomcat -C tomcat安装目录
```

# 3.创建 systemd Service 文件

创建systemd service 文件来管理Tomcat 进程。

在/etc/systemd/system/ 下创建tomcat.service
```bash
sudo vim /etc/systemd/system/tomcat.service
```

添加以下内容：
```bash
[Unit]
Description=Apache Tomcat Web Server
After=network.target
 
[Service]
Type=forking
 
Environment=JAVA_HOME=jdk安装路径
Environment=CATALINA_PID=tomcat安装路径/temp/tomcat.pid
Environment=CATALINA_HOME=tomcat安装路径
Environment=CATALINA_BASE=tomcat安装路径
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'
 
ExecStart=tomcat安装路径/bin/startup.sh
ExecStop=tomcat安装路径/bin/shutdown.sh
 
User=root  //用户
Group=root //用户组
UMask=0007
RestartSec=15
Restart=always
 
[Install]
WantedBy=multi-user.target
```

[Unit] 表示这是基础信息 
Description 是描述
After 是在那个服务后面启动，一般是网络服务启动后启动

[Service] 表示这里是服务信息 
Type 是服务类型
PIDFile 是服务的pid文件路径， 开启后，必须在tomcat的bin/catalina.sh中加入CATALINA_PID参数
ExecStart 是启动服务的命令
ExecReload 是重启服务的命令
ExecStop 是停止服务的指令

[Install] 表示这是是安装相关信息 
WantedBy 是以哪种方式启动：multi-user.target表明当系统以多用户方式（默认的运行级别）启动时，这个服务需要被自动运行。

保存文件，刷新配置，让systemctl能识别。
```bash
sudo systemctl daemon-reload
```

启动Tomcat，并检查Tomcat的状态:
```bash
sudo systemctl start tomcat
sudo systemctl status tomcat
```

允许开机自动启动:
```bash
sudo systemctl enable tomcat
```

# 4.允许Tomcat通过防火墙

Tomcat默认端口为 8080, 要允许这个端口通过防火墙，命令如下：
```bash
sudo ufw allow 8080
```