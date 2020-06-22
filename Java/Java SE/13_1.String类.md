# String的原理
![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200622132704.png)
由`=`创建的String对象,会存放在堆的字符串常量池中

# 比较方法
1. equals()类的,返回的是boolean值,比较的是字符串的内容
~~~java
boolean equals(Object anObject)；
boolean contentEquals(StringBuffer sb)；
boolean contentEquals(CharSequence cs)；
boolean equalsIgnoreCase(String anotherString)；
~~~
~~~java
boolean regionMatches(int toffset, String other, int ooffset,int len)  //局部匹配
boolean regionMatches(boolean ignoreCase, int toffset,String other, int ooffset, int len)   //局部匹配
~~~

2. compareTo()类
~~~java
int compareTo(String anotherString)；
int compareToIgnoreCase(String str)；
~~~
按字典顺序比较两个字符串。该比较基于字符串中各个字符的 Unicode 值
两个字符串不同的情况:
          * 在某个索引处的字符不同
          *  长度不同
          *  以上两种兼有
* 如 果它们在一个或多个索引位置上的字符不同，假设 k 是这类索引的最小值；则在位置 k 上具有较小值的那个字符串（使用 < 运算符确定），其字典顺序在其他字符串之前。在这种情况下，`compareTo` 返回这两个字符串在位置 k 处两个char 值的差，即值：
    ~~~java
    this.charAt(k)-anotherString.charAt(k)
    ~~~
* 如果长度不同，`compareTo` 返回这两个字符串长度的差，即值：
    ~~~java
    this.length()-anotherString.length()
    ~~~

3. ==
判断的是否是同一个对象

# str.lastIndexOf(Stringname)
获取指定子字符串的最后一次出现的位置

# 从字符串中删除指定的字符
~~~java
 public static String removeCharAt(String s, int pos) {
        return s.substring(0, pos) + s.substring(pos + 1);
    }
~~~

# str.replace()
用一个字符串替换另一个字符串中的子串
~~~java
str.replaceFirst() // 替换匹配的第一个
str.replaAll() // 替换匹配的全部
~~~

# 如何反转倒置字符串
1. 使用`StringBuffer(String string)`方法缓冲输入`String`，反转缓冲区，然后使用`toString()`方法将缓冲区转换成String。
~~~java
String reverse = new StringBuffer(string).reverse().toString();
~~~
2. 手动实现 先把String转化为charArray数组,再从数组的最后到第一个依次输出
~~~java
String str1 = "www.baidu.com";
        char[] chars = str1.toCharArray();
        for (int i=chars.length-1;i>=0;i--){
            System.out.print(chars[i]);
        }
~~~

# 如何在字符串中查找词组
1. `indexOf()`方法在`String`对象中搜索单词，该方法返回字符串中的单词的位置索引(如果找到)值。 否则返回`-1`。
    ~~~java
    str.indexOf();
    ~~~
2. `contains()`方法在`String`对象中搜索单词,若有该单词返回`true`,若无则返回`false`
    ~~~java
    str.contains();
    ~~~

# 如何拆分/分割字符串
`split(string)`方法将字符串分割成多个子字符串
~~~java
str.split(delimeter);
~~~

# 如何字符串转转换为大写
使用`String`类的`toUpperCase()`方法将字符串的大小写更改为大写。
~~~java
str.toUpperCase();
~~~

# 如何匹配字符串区域
`regionMatches()`方法确定两个字符串中的区域匹配。
~~~java
public class StringRegionMatch {
    public static void main(String[] args) {
        String first_str = "Welcome to IBM";
        String second_str = "I work with IBM";

        boolean match = first_str.regionMatches(11, second_str, 12, 3);
        System.out.println("first_str[11->14] == " + "second_str[12 -> 15]: "
                + match);
    }
}
~~~
以下是几个数字参数的说明：
 * 11 - 是比较开始的源字符串中的索引号
 * second_str - 是目标字符串
 * 12是从目标字符串开始比较的索引号
 * 3是要比较的字符数

执行上面示例代码，得到以下结果 
~~~shell
first_str[11->14] == second_str[12 -> 15]: true
~~~