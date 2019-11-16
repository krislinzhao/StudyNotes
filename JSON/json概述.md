JSON      JavaScript 对象表示法（JavaScript Object Notation） 是一种存储数据的方式。

# JSON的用途

* 使用基于JavaScript的应用程序，其中包括浏览器扩展和网站
* 使用JSON格式序列化和结构化的数据传输网络连接
* 这主要用于服务器和Web应用程序之间的数据传输
* Web服务和API采用JSON格式提供公共数据

# JSON的特点

* 易于读写
* 轻量级的基于文本的交换格式
* 独立语言

# JSON简单的例子

示例显示图书信息存储使用JSON考虑语言的书籍和有版本：
{
    "book": [
    {
       "id":"01",
       "language": "Java",
       "edition": "third",
       "author": "Herbert Schildt"
    },
    {
       "id":"07",
       "language": "C++",
       "edition": "second"
       "author": "E.Balagurusamy"
    }]
}

# JSON语法

* 名称/值对数据表示
* 大括号持有的对象和每个名称后跟“：”（冒号），名称/值对的分离，（逗号）
* 方括号持有数组和值，（逗号）分隔。

## JSON支持以下两种数据结构：

1. 名称/值对的集合: 此数据结构支持由不同的编程语言。
2. 值的有序列表: 它包括数组，列表，向量或序列等。


