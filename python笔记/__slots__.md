# 使用__slots__

## 为什么引入__slots__
python是动态语言，正常情况下，当我们定义了一个class，创建了一个class的实例后，我们可以给该实例绑定任何大物属性和方法。
**注意：**
* 给实例绑定的方法，对另一个实例是不起作用的
* 为了给所用实例绑定方法，可以给class绑定方法
  
但是，如果我们想要限制实例的属性怎么办？比如，只允许对Student实例添加name和age属性。


## 怎么使用__slots__
为了达到限制的目的，Python允许在定义class的时候，定义一个特殊的__slots__变量，来限制该class实例能添加的属性：
~~~python
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
~~~
然后，我们试试：
~~~python
>>> s = Student() # 创建新的实例
>>> s.name = 'Michael' # 绑定属性'name'
>>> s.age = 25 # 绑定属性'age'
>>> s.score = 99 # 绑定属性'score'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'score'
~~~
由于'**score**'没有被放到__slots__中，所以不能绑定**score**属性，试图绑定 **score**将得到 **AttributeError**的错误。

使用__slots__要注意,__slots__定义的属性仅对当前类实例起作用，对继承的子类是不起作用的：
~~~python
>>> class GraduateStudent(Student):
...     pass
...
>>> g = GraduateStudent()
>>> g.score = 9999
~~~
除非在子类中也定义__slots__，这样，子类实例允许定义的属性就是自身的__slots__ 加上父类的__slots__。


**注意：**
~~~python
class Student(object):
	__slots__ = ('name','score','set_age')

stu = Student()
stu.name = 'Zhangsan'
stu.score = 90
 #stu.age = 20  无法添加该属性

print(stu.name) #正常打印
print(stu.score) #正常打印
 #print(stu.age) 无法打印

Student.age = 20

print(stu.age) #正常打印
~~~
**通过上面我们发现使用__slots__限制的仅仅是类的实例的属性或者方法的动态添加，类本身的属性的添加不受__slots__的限制。**