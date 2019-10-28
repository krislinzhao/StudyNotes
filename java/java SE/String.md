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

