# 1.a, b = b, a + b  等价于  (a, b) = (b, a + b)

1.在赋值之前先计算等号右边的值


2.计算时的结果不参与二次计算，而是使用原来的值进行计算


等价于下的表达：


~~~python
tmp = a
a = b
b = tmp + b
~~~

# 2.python的内置函数

​    \- 数学相关: abs / divmod / pow / round / min / max / sum

​    \- 序列相关: len / range / next / filter / map / sorted / slice / reversed

​    \- 类型转换: chr / ord / str / bool / int / float / complex / bin / oct / hex

​    \- 数据结构: dict / list / set / tuple

​    \- 其他函数: all / any / id / input / open / print / type

# 3.python常用模块

​    \- 运行时服务相关模块: copy / pickle / sys / ...

​    \- 数学相关模块: decimal / math / random / ...

​    \- 字符串处理模块: codecs / re / ...

​    \- 文件处理相关模块: shutil / gzip / ...

​    \- 操作系统服务相关模块: datetime / os / time / logging / io / ...

​    \- 进程和线程相关模块: multiprocessing / threading / queue

​    \- 网络应用相关模块: ftplib / http / smtplib / urllib / ...

​    \- Web编程相关模块: cgi / webbrowser

​    \- 数据处理和编码模块: base64 / csv / html.parser / json / xml / ...

# 4.python中的private和protected

1、_xxx     不能用于’from module import *’ 以单下划线开头的表示的是protected类型的变量。即保护类型只能允许其本身与子类进行访问。

2、__xxx    双下划线的表示的是私有类型的变量。只能是允许这个类本身进行访问了。连子类也不可以

3、__xxx___ 定义的是特列方法。像__init__之类的

