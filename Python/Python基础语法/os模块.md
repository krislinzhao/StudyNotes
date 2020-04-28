# os模块
Python中的os模块封装了常见的文件和目录操作
[官方文档](https://docs.python.org/3/library/os.path.html)
部分常见用法:
方法|说明
---|---
os.mkdir|创建目录
os.rmdir|删除目录
os.rename|重命名
os.remove|删除文件
os.getcwd|获取当前工作路径
os.walk|遍历目录
os.path.join|连接目录和文件名
os.path.split|分割文件名和目录
os.path.abspath|获取绝对路径
os.path.dirname|获取路径
os.path.basename|获取文件名或文件夹名
os.path.splitext|分离文件名和扩展名
os.path.isfile|判断给出的路径是否是有一个文件
os.path.isdir|判断给出的路径是否是一个目录

# 例子
目录结构
~~~python
Users/ethan
└── coding
    └── python
        ├── hello.py    - 文件
        └── web         - 目录
~~~
* os.path.abspath:获取文件或目录的绝对路径
~~~python
$ pwd
/Users/ethan/coding/python
$ python
>>> import os                          # 记得导入 os 模块
>>> os.path.abspath('hello.py')
'/Users/ethan/coding/python/hello.py'
>>> os.path.abspath('web')
'/Users/ethan/coding/python/web'
>>> os.path.abspath('.')                # 当前目录的绝对路径
'/Users/ethan/coding/python'
~~~
* os.path.dirname:获取文件或文件夹的路径
~~~python
>>> os.path.dirname('/Users/ethan/coding/python/hello.py')
'/Users/ethan/coding/python'
>>> os.path.dirname('/Users/ethan/coding/python/')
'/Users/ethan/coding/python'
>>> os.path.dirname('/Users/ethan/coding/python')
'/Users/ethan/coding'
~~~
* os.path.basename:获取文件名或文件夹名
~~~python
>>> os.path.basename('/Users/ethan/coding/python/hello.py')
'hello.py'
>>> os.path.basename('/Users/ethan/coding/python/')
''
>>> os.path.basename('/Users/ethan/coding/python')
'python'
~~~
* os.path.splitext:分离文件名和扩展名
~~~python
>>> os.path.splitext('/Users/ethan/coding/python/hello.py')
('/Users/ethan/coding/python/hello', '.py')
>>> os.path.splitext('/Users/ethan/coding/python')
('/Users/ethan/coding/python', '')
>>> os.path.splitext('/Users/ethan/coding/python/')
('/Users/ethan/coding/python/', '')
~~~
* os.path.split:分离目录与文件名
~~~python
>>> os.path.split('/Users/ethan/coding/python/hello.py')
('/Users/ethan/coding/python', 'hello.py')
>>> os.path.split('/Users/ethan/coding/python/')
('/Users/ethan/coding/python', '')
>>> os.path.split('/Users/ethan/coding/python')
('/Users/ethan/coding', 'python')
~~~
* os.path.isfile/os.path.isdir:是不是文件或目录
~~~python
>>> os.path.isfile('/Users/ethan/coding/python/hello.py')
True
>>> os.path.isdir('/Users/ethan/coding/python/')
True
>>> os.path.isdir('/Users/ethan/coding/python')
True
>>> os.path.isdir('/Users/ethan/coding/python/hello.py')
False
~~~
* os.walk
os.walk是遍历目录常用的模块，它返回一个包含3个元素的元祖:(dirpath,dirnames,filenames)。dirpath是以string字符串形式返回该目录下所有的绝对路径；dirnames是以列表list形式返回每一个绝对路径下的文件夹名字；filenames是以列表list形式返回该路径下所有文件名字
~~~python
>>> for root, dirs, files in os.walk('/Users/ethan/coding'):
...     print root
...     print dirs
...     print files
...
/Users/ethan/coding
['python']
[]
/Users/ethan/coding/python
['web2']
['hello.py']
/Users/ethan/coding/python/web2
[]
[]
~~~