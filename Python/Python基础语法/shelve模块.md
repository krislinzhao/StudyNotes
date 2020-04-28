# shelve介绍
[官方文档](https://docs.python.org/3/library/shelve.html)

shelve是python中一种存储结构化数据的模块，用法跟字典相似，以键值对的形式储存。
windows下shelf文件是生成三个，两个可读一个不可读，linux下生成一个二进制文件，不可读
## 基本使用方法
### 1.引入模块
~~~python
import shelve
~~~
### 2.创建数据文件
~~~python
1. with shelve.open(filename) as object:
2. object = shelve.open(filename)
~~~
两种文件操作的方式均可以。不难看出，对shelf文件数据的读写是不用指定文件打开方式的，直接open就可以了，跟json比起来较方便，尽管只适用于python程序。对于shelf内容的操作，不用像文件操作一样，需要readline等先对数据做一波处理，也不用像pickle/json一样需要load/dump，更不需要像pformat一样将数据结构输出为字符串，格式化写入程序文件进行调用，对于shelf的操作，如上object即可视为字典变量，操作同字典
### 3.shelf的键值只能存储基础的数据结构，例如file类型就不行
### 4.shelf虽然同字典很类似，但是略有差别，key只能是字符串类型，不能是数值
# 问题(注意)
## 问题1
~~~python
import shelve
with shelve.open('test1') as b:
    b = {
        'b':{'d':5000},
        'c':{'d':5000}
    }
    if 'b' in b:
        print('yes')
    print(list(b.keys()))
    print(b['c']['d'])

输出:
yes
['b', 'c']
5000
~~~
但是当把它出文件中读出来的时候就会出错
~~~python
import shelve
with shelve.open('test1') as b:
    if 'b' in b:
        print('yes')
    print(list(b.keys()))
    print(b['c']['d'])

输出:
[]
---------------------------------------------------------------------------
KeyError                                  Traceback (most recent call last)
D:\Anaconda3\lib\shelve.py in __getitem__(self, key)
    110         try:
--> 111             value = self.cache[key]
    112         except KeyError:

KeyError: 'c'

During handling of the above exception, another exception occurred:

KeyError                                  Traceback (most recent call last)
<ipython-input-12-bbc518462be6> in <module>
      4         print('yes')
      5     print(list(b.keys()))
----> 6     print(b['c']['d'])

D:\Anaconda3\lib\shelve.py in __getitem__(self, key)
    111             value = self.cache[key]
    112         except KeyError:
--> 113             f = BytesIO(self.dict[key.encode(self.keyencoding)])
    114             value = Unpickler(f).load()
    115             if self.writeback:

D:\Anaconda3\lib\dbm\dumb.py in __getitem__(self, key)
    151             key = key.encode('utf-8')
    152         self._verify_open()
--> 153         pos, siz = self._index[key]     # may raise KeyError
    154         with _io.open(self._datfile, 'rb') as f:
    155             f.seek(pos)

KeyError: b'c'

~~~
检查了一下生成的test1的三个文件都是空的。
这是因为，shelf键值对进行数据结构存储时，只能严格按照key=value这样的形式存储，而不能将shelf视为字典直接进行初始化
~~~python
import shelve
with shelve.open('test2') as b:
    b['b'] = {'d':5000}
    b['c'] = {'d':5000}
~~~
~~~python
with shelve.open('test2') as b:
    if 'b' in b:
        print('yes')
    print(list(b.keys()))
    print(b['c']['d'])

输出:
yes
['b', 'c']
5000
~~~
## 问题2
shelf默认初始化存储数据之后是不进行会写，因此之后的数据变动不会被写入，因此只要指定writeback参数为`True`即可修改数据
~~~python
import shelve
with shelve.open('test3',writeback=True) as b:
    b['b'] = {'d':5000}
    b['c'] = {'d':5000}
    b['c']['d'] = 1000
    if 'b' in b:
        print('yes')
    print (list(b.keys()))
    print(b['c']['d'])

输出:
yes
['b', 'c']
1000
~~~