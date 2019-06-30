# supper()函数
## 描述
1. super() 函数是用于调用父类(超类)的一个方法。
2. super 是用来解决多重继承问题的，直接用类名调用父类方法在使用单继承的时候没问题，但是如果使用多继承，会涉及到查找顺序（MRO）、重复调用（钻石继承）等种种问题。
MRO 就是类的方法解析顺序表, 其实也就是继承父类方法时的顺序表。
##语法(python3)
~~~python
super().xxx
~~~


## 例子


![](../images/菱形继承.jpg)
使用 super() 可以很好地避免构造函数被调用两次。
~~~python
class A():
    def __init__(self):
        print('enter A')
        print('leave A')


class B(A):
    def __init__(self):
        print('enter B')
        super().__init__()
        print('leave B')


class C(A):
    def __init__(self):
        print('enter C')
        super().__init__()
        print('leave C')


class D(B, C):
    def __init__(self):
        print('enter D')
        super().__init__()
        print('leave D')


d = D()
~~~
执行结果是：
~~~python
enter D
enter B
enter C
enter A
leave A
leave C
leave B
leave D
~~~