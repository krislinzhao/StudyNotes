- [1. 基于apt源安装](#1-基于apt源安装)
	- [1.1 安装](#11-安装)
	- [1.2 测试安装是否成功](#12-测试安装是否成功)
	- [1.3 卸载](#13-卸载)
		- [1. 停止nginx服务](#1-停止nginx服务)
		- [2. 删除nginx，-purge包括配置文件](#2-删除nginx-purge包括配置文件)
		- [3. 移除全部不使用的软件包](#3-移除全部不使用的软件包)
		- [4. 罗列出与nginx相关的软件并删除](#4-罗列出与nginx相关的软件并删除)
		- [5. 查看nginx正在运行的进程，如果有就kill掉](#5-查看nginx正在运行的进程如果有就kill掉)
- [2. 通过源码包编译安装](#2-通过源码包编译安装)
	- [1. 安装各种依赖库](#1-安装各种依赖库)
	- [2. 安装Nginx](#2-安装nginx)
	- [3. 配置软链接](#3-配置软链接)
	- [4. 配置开机启动服务](#4-配置开机启动服务)

此安装过程是在ubuntu18下完成的。

# 1. 基于apt源安装

## 1.1 安装

```shell
// 更新包
sudo apt-get update
// 下载安装nginx
sudo apt-get install nginx
```

Ubuntu安装之后的文件结构大致为：

- 所有的配置文件都在/etc/nginx下，并且每个虚拟主机已经安排在了/etc/nginx/sites-available下
- 程序文件在/usr/sbin/nginx
- 日志放在了/var/log/nginx中
- 并已经在/etc/init.d/下创建了启动脚本nginx
- 默认的虚拟主机的目录设置在了/var/www/nginx-default (有的版本 默认的虚拟主机的目录设置在了/var/www, 请参考/etc/nginx/sites-available里的配置)

其实从上面的根目录文件夹可以知道，Linux系统的配置文件一般放在/etc，日志一般放在/var/log，运行的程序一般放在/usr/sbin或者/usr/bin。

当然，如果要更清楚Nginx的配置项放在什么地方，可以打开/etc/nginx/nginx.conf

然后通过这种方式安装的，会自动创建服务，会自动在/etc/init.d/nginx新建服务脚本，然后就可以使用一下命令来启动

```shell
sudo service nginx {start|stop|restart|reload|force-reload|status|configtest|rotate|upgrade}
```

脚本如下：

```shell
#!/bin/sh

### BEGIN INIT INFO
# Provides:	  nginx
# Required-Start:    $local_fs $remote_fs $network $syslog $named
# Required-Stop:     $local_fs $remote_fs $network $syslog $named
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: starts the nginx web server
# Description:       starts nginx using start-stop-daemon
### END INIT INFO

PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
DAEMON=/usr/sbin/nginx
NAME=nginx
DESC=nginx

# Include nginx defaults if available
if [ -r /etc/default/nginx ]; then
	. /etc/default/nginx
fi

STOP_SCHEDULE="${STOP_SCHEDULE:-QUIT/5/TERM/5/KILL/5}"

test -x $DAEMON || exit 0

. /lib/init/vars.sh
. /lib/lsb/init-functions

# Try to extract nginx pidfile
PID=$(cat /etc/nginx/nginx.conf | grep -Ev '^\s*#' | awk 'BEGIN { RS="[;{}]" } { if ($1 == "pid") print $2 }' | head -n1)
if [ -z "$PID" ]; then
	PID=/run/nginx.pid
fi

if [ -n "$ULIMIT" ]; then
	# Set ulimit if it is set in /etc/default/nginx
	ulimit $ULIMIT
fi

start_nginx() {
	# Start the daemon/service
	#
	# Returns:
	#   0 if daemon has been started
	#   1 if daemon was already running
	#   2 if daemon could not be started
	start-stop-daemon --start --quiet --pidfile $PID --exec $DAEMON --test > /dev/null \
		|| return 1
	start-stop-daemon --start --quiet --pidfile $PID --exec $DAEMON -- \
		$DAEMON_OPTS 2>/dev/null \
		|| return 2
}

test_config() {
	# Test the nginx configuration
	$DAEMON -t $DAEMON_OPTS >/dev/null 2>&1
}

stop_nginx() {
	# Stops the daemon/service
	#
	# Return
	#   0 if daemon has been stopped
	#   1 if daemon was already stopped
	#   2 if daemon could not be stopped
	#   other if a failure occurred
	start-stop-daemon --stop --quiet --retry=$STOP_SCHEDULE --pidfile $PID --name $NAME
	RETVAL="$?"
	sleep 1
	return "$RETVAL"
}

reload_nginx() {
	# Function that sends a SIGHUP to the daemon/service
	start-stop-daemon --stop --signal HUP --quiet --pidfile $PID --name $NAME
	return 0
}

rotate_logs() {
	# Rotate log files
	start-stop-daemon --stop --signal USR1 --quiet --pidfile $PID --name $NAME
	return 0
}

upgrade_nginx() {
	# Online upgrade nginx executable
	# http://nginx.org/en/docs/control.html
	#
	# Return
	#   0 if nginx has been successfully upgraded
	#   1 if nginx is not running
	#   2 if the pid files were not created on time
	#   3 if the old master could not be killed
	if start-stop-daemon --stop --signal USR2 --quiet --pidfile $PID --name $NAME; then
		# Wait for both old and new master to write their pid file
		while [ ! -s "${PID}.oldbin" ] || [ ! -s "${PID}" ]; do
			cnt=`expr $cnt + 1`
			if [ $cnt -gt 10 ]; then
				return 2
			fi
			sleep 1
		done
		# Everything is ready, gracefully stop the old master
		if start-stop-daemon --stop --signal QUIT --quiet --pidfile "${PID}.oldbin" --name $NAME; then
			return 0
		else
			return 3
		fi
	else
		return 1
	fi
}

case "$1" in
	start)
		log_daemon_msg "Starting $DESC" "$NAME"
		start_nginx
		case "$?" in
			0|1) log_end_msg 0 ;;
			2)   log_end_msg 1 ;;
		esac
		;;
	stop)
		log_daemon_msg "Stopping $DESC" "$NAME"
		stop_nginx
		case "$?" in
			0|1) log_end_msg 0 ;;
			2)   log_end_msg 1 ;;
		esac
		;;
	restart)
		log_daemon_msg "Restarting $DESC" "$NAME"

		# Check configuration before stopping nginx
		if ! test_config; then
			log_end_msg 1 # Configuration error
			exit $?
		fi

		stop_nginx
		case "$?" in
			0|1)
				start_nginx
				case "$?" in
					0) log_end_msg 0 ;;
					1) log_end_msg 1 ;; # Old process is still running
					*) log_end_msg 1 ;; # Failed to start
				esac
				;;
			*)
				# Failed to stop
				log_end_msg 1
				;;
		esac
		;;
	reload|force-reload)
		log_daemon_msg "Reloading $DESC configuration" "$NAME"

		# Check configuration before stopping nginx
		#
		# This is not entirely correct since the on-disk nginx binary
		# may differ from the in-memory one, but that's not common.
		# We prefer to check the configuration and return an error
		# to the administrator.
		if ! test_config; then
			log_end_msg 1 # Configuration error
			exit $?
		fi

		reload_nginx
		log_end_msg $?
		;;
	configtest|testconfig)
		log_daemon_msg "Testing $DESC configuration"
		test_config
		log_end_msg $?
		;;
	status)
		status_of_proc -p $PID "$DAEMON" "$NAME" && exit 0 || exit $?
		;;
	upgrade)
		log_daemon_msg "Upgrading binary" "$NAME"
		upgrade_nginx
		log_end_msg $?
		;;
	rotate)
		log_daemon_msg "Re-opening $DESC log files" "$NAME"
		rotate_logs
		log_end_msg $?
		;;
	*)
		echo "Usage: $NAME {start|stop|restart|reload|force-reload|status|configtest|rotate|upgrade}" >&2
		exit 3
		;;
esac

```

还有一个好处，创建好的文件由于放在/usr/sbin目录下，所以能直接在终端中使用nginx命令而无需指定路径。

## 1.2 测试安装是否成功

在命令行中输入：

```shell
sudo nginx -t
```

窗口显示：

```shell
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

在浏览器中输入ip地址：

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200519123550.png)

## 1.3 卸载

### 1. 停止nginx服务

```shell
sudo service nginx stop
```

### 2. 删除nginx，-purge包括配置文件

```shell
sudo apt-get --purge remove nginx
```

### 3. 移除全部不使用的软件包

```shell
sudo apt-get autoremove
```

### 4. 罗列出与nginx相关的软件并删除

```shell
dpkg --get-selections|grep nginx
sudo apt-get --purge remove nginx
sudo apt-get --purge remove nginx-common
sudo apt-get --purge remove nginx-core
```

### 5. 查看nginx正在运行的进程，如果有就kill掉

```shell
ps -ef |grep nginx
sudo kill -9 XXX
```

# 2. 通过源码包编译安装

这种方式可以自定安装指定的模块以及最新的版本。方式更灵活。

官方下载页面：http://nginx.org/en/download.html

configure配置文件详解：http://nginx.org/en/docs/configure.html

## 1. 安装各种依赖库

 安装gcc g++的依赖库

```shell
sudo apt-get install build-essential
sudo apt-get install libtool
```

安装pcre依赖库（http://www.pcre.org/）

```shell
sudo apt-get update
sudo apt-get install libpcre3 libpcre3-dev
```

安装zlib依赖库（[http://www.zlib.net](http://www.zlib.net/)）

```shell
sudo apt-get install zlib1g-dev
```

安装SSL依赖库（18.04默认已经安装了）

```shell
sudo apt-get install openssl
```

## 2. 安装Nginx

```shell
#下载最新版本：
wget http://nginx.org/download/nginx-1.13.6.tar.gz
#解压：
tar -zxvf nginx-1.13.6.tar.gz
#进入解压目录：
cd nginx-1.13.6
#配置：
./configure --prefix=/usr/local/nginx 
#编译：
make
#安装：
sudo make install
#启动：
sudo /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
注意：-c 指定配置文件的路径，不加的话，nginx会自动加载默认路径的配置文件，可以通过-h查看帮助命令。
#查看进程：
ps -ef | grep nginx
```

## 3. 配置软链接

```shell
sudo ln -s /usr/local/nginx/sbin/nginx /usr/bin/nginx
```

现在就可以不用路径直接输入nginx启动。

## 4. 配置开机启动服务

在/etc/init.d/下创建nginx文件，sudo vim /etc/init.d/nginx，内容如下：

```shell
#!/bin/sh

### BEGIN INIT INFO
# Provides:	  nginx
# Required-Start:    $local_fs $remote_fs $network $syslog $named
# Required-Stop:     $local_fs $remote_fs $network $syslog $named
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: starts the nginx web server
# Description:       starts nginx using start-stop-daemon
### END INIT INFO

PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
DAEMON=/usr/sbin/nginx
NAME=nginx
DESC=nginx

# Include nginx defaults if available
if [ -r /etc/default/nginx ]; then
	. /etc/default/nginx
fi

STOP_SCHEDULE="${STOP_SCHEDULE:-QUIT/5/TERM/5/KILL/5}"

test -x $DAEMON || exit 0

. /lib/init/vars.sh
. /lib/lsb/init-functions

# Try to extract nginx pidfile
PID=$(cat /etc/nginx/nginx.conf | grep -Ev '^\s*#' | awk 'BEGIN { RS="[;{}]" } { if ($1 == "pid") print $2 }' | head -n1)
if [ -z "$PID" ]; then
	PID=/run/nginx.pid
fi

if [ -n "$ULIMIT" ]; then
	# Set ulimit if it is set in /etc/default/nginx
	ulimit $ULIMIT
fi

start_nginx() {
	# Start the daemon/service
	#
	# Returns:
	#   0 if daemon has been started
	#   1 if daemon was already running
	#   2 if daemon could not be started
	start-stop-daemon --start --quiet --pidfile $PID --exec $DAEMON --test > /dev/null \
		|| return 1
	start-stop-daemon --start --quiet --pidfile $PID --exec $DAEMON -- \
		$DAEMON_OPTS 2>/dev/null \
		|| return 2
}

test_config() {
	# Test the nginx configuration
	$DAEMON -t $DAEMON_OPTS >/dev/null 2>&1
}

stop_nginx() {
	# Stops the daemon/service
	#
	# Return
	#   0 if daemon has been stopped
	#   1 if daemon was already stopped
	#   2 if daemon could not be stopped
	#   other if a failure occurred
	start-stop-daemon --stop --quiet --retry=$STOP_SCHEDULE --pidfile $PID --name $NAME
	RETVAL="$?"
	sleep 1
	return "$RETVAL"
}

reload_nginx() {
	# Function that sends a SIGHUP to the daemon/service
	start-stop-daemon --stop --signal HUP --quiet --pidfile $PID --name $NAME
	return 0
}

rotate_logs() {
	# Rotate log files
	start-stop-daemon --stop --signal USR1 --quiet --pidfile $PID --name $NAME
	return 0
}

upgrade_nginx() {
	# Online upgrade nginx executable
	# http://nginx.org/en/docs/control.html
	#
	# Return
	#   0 if nginx has been successfully upgraded
	#   1 if nginx is not running
	#   2 if the pid files were not created on time
	#   3 if the old master could not be killed
	if start-stop-daemon --stop --signal USR2 --quiet --pidfile $PID --name $NAME; then
		# Wait for both old and new master to write their pid file
		while [ ! -s "${PID}.oldbin" ] || [ ! -s "${PID}" ]; do
			cnt=`expr $cnt + 1`
			if [ $cnt -gt 10 ]; then
				return 2
			fi
			sleep 1
		done
		# Everything is ready, gracefully stop the old master
		if start-stop-daemon --stop --signal QUIT --quiet --pidfile "${PID}.oldbin" --name $NAME; then
			return 0
		else
			return 3
		fi
	else
		return 1
	fi
}

case "$1" in
	start)
		log_daemon_msg "Starting $DESC" "$NAME"
		start_nginx
		case "$?" in
			0|1) log_end_msg 0 ;;
			2)   log_end_msg 1 ;;
		esac
		;;
	stop)
		log_daemon_msg "Stopping $DESC" "$NAME"
		stop_nginx
		case "$?" in
			0|1) log_end_msg 0 ;;
			2)   log_end_msg 1 ;;
		esac
		;;
	restart)
		log_daemon_msg "Restarting $DESC" "$NAME"

		# Check configuration before stopping nginx
		if ! test_config; then
			log_end_msg 1 # Configuration error
			exit $?
		fi

		stop_nginx
		case "$?" in
			0|1)
				start_nginx
				case "$?" in
					0) log_end_msg 0 ;;
					1) log_end_msg 1 ;; # Old process is still running
					*) log_end_msg 1 ;; # Failed to start
				esac
				;;
			*)
				# Failed to stop
				log_end_msg 1
				;;
		esac
		;;
	reload|force-reload)
		log_daemon_msg "Reloading $DESC configuration" "$NAME"

		# Check configuration before stopping nginx
		#
		# This is not entirely correct since the on-disk nginx binary
		# may differ from the in-memory one, but that's not common.
		# We prefer to check the configuration and return an error
		# to the administrator.
		if ! test_config; then
			log_end_msg 1 # Configuration error
			exit $?
		fi

		reload_nginx
		log_end_msg $?
		;;
	configtest|testconfig)
		log_daemon_msg "Testing $DESC configuration"
		test_config
		log_end_msg $?
		;;
	status)
		status_of_proc -p $PID "$DAEMON" "$NAME" && exit 0 || exit $?
		;;
	upgrade)
		log_daemon_msg "Upgrading binary" "$NAME"
		upgrade_nginx
		log_end_msg $?
		;;
	rotate)
		log_daemon_msg "Re-opening $DESC log files" "$NAME"
		rotate_logs
		log_end_msg $?
		;;
	*)
		echo "Usage: $NAME {start|stop|restart|reload|force-reload|status|configtest|rotate|upgrade}" >&2
		exit 3
		;;
esac
```

```shell
#设置服务脚本有执行权限
sudo chmod +x /etc/init.d/nginx
#注册服务cd /etc/init.d/
sudo update-rc.d nginx defaults
```

现在基本上就可以开机启动了，常用的命令如下：

```shell
sudo service nginx {start|stop|restart|reload|force-reload|status|configtest|rotate|upgrade}
```