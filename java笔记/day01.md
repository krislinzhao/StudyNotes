# day01

<a name="8O9b3"></a>
# java标识符
(1)标识符：给类，接口，方法或者变量起名字的符号<br />
(2)组成规则：<br />A：英文字母大小写<br />B：数字<br />C：_和$<br />(3)注意事项：<br />A：不能以数字开头<br />B：不能是java中的关键字<br />C：区分大小写<br />(4)常见命名方式：<br />A：包 其实就是文件夹，用于解决相同类名问题<br />全部小写<br />单级：com<br />多级：cn.itcast<br />B：类或者接口<br />一个单词：首字母大写<br />多个单词：每个字母的首字母大写<br />
~~~java
HelloWorld，MyAllStudentNames()
~~~
<br />C：方法或者变量<br />一个单词：全部小写<br />多个单词：从第二个单词开始，每个单词的首字母大写<br />
~~~java
myName，showAllStudentName()
~~~
<br />D：常量<br />一个单词：全部大写<br />多个单词：每个单词都大写，用_连接<br />
~~~java
STUDENT_MAX_AGE
~~~