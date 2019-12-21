# 1.去官网下载对应的linux版本jdk
[jdk网址](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

# 2.解压到相应的目录
```bash
tar -xzf 下载的linux版jdk -C jdk安装目录
```

# 3.配置环境变量
```bash
sudo vi /etc/profile
```

```bash
#set java environment
export JAVA_HOME=jdk安装目录
export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib
export PATH=$JAVA_HOME/bin:$PATH
```

保存后退出，加载环境变量:
```bash
source /etc/profile
```

# 4.查看java版本
```bash
java -version
```