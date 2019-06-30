# platform模块
platform模块给我们提供了很多方法去获取操作系统的信息
~~~python
import platform
platform.system()          #获取操作系统名称
platform.platform()        #获取操作系统名称及版本号
platform.version()         #获取操作系统版本号
platform.architecture()    #获取操作系统的位数，('32bit', 'ELF')
platform.machine()         #计算机类型，'i686'
platform.node()            #计算机的网络名称，'XF654'
platform.processor()       #计算机处理器信息，''i686'
platform.uname()           #包含上面所有的信息汇总
~~~
还可以获得计算机中python的一些信息
~~~python
import platform
platform.python_build()             #python编译号(default)和日期.
platform.python_compiler()          #python编译器信息
platform.python_branch()            #python分支(子版本信息),一般为空.
platform.python_implementation()    #python安装履行方式,如CPython, Jython, Pypy, IronPython(.net)等
platform.python_revision()          #python类型修改版信息,一般为空.
platform.python_version()           #python版本号
platform.python_version_tuple()     #python版本号分割后的tuple.
~~~
