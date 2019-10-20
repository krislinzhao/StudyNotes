在虚拟环境中将当前虚拟环境中的依赖包以及版本号生成至文件中 
pip freeze >requirements.txt
在新的虚拟环境中，生成项目的运行环境的完全副本
pip install -r requirements.txt
# 指定下载源
pip install -i https://pypi.douban.com/simple/ --trusted-host pypi.douban.com -r requirements.txt


flask中mysql数据库的创建
1.生成数据库
python manage.py db init		# 创建迁移仓库，首次使用
python manage.py db migrate		# 创建迁移脚本
python manage.py db upgrade		# 把迁移应用到数据库中

2.启动服务
python manage.py runserver

过滤器
注意：自定义的过滤器名称如果和内置的过滤器重名，会覆盖内置的过滤器
字符串操作：
safe：禁用转义；
<p>{{ '<em>hello</em>' | safe }}</p>
capitalize：把变量值的首字母转成大写，其余字母转小写；
<p>{{ 'hello' | capitalize }}</p>
lower：把值转成小写；
<p>{{ 'HELLO' | lower }}</p>
upper：把值转成大写；
<p>{{ 'hello' | upper }}</p>
title：把值中的每个单词的首字母都转成大写；
<p>{{ 'hello' | title }}</p>
trim：把值的首尾空格去掉；
<p>{{ ' hello world ' | trim }}</p>
reverse:字符串反转；
<p>{{ 'olleh' | reverse }}</p>
format:格式化输出；
<p>{{ '%s is %d' | format('name',17) }}</p>
striptags：渲染之前把值中所有的HTML标签都删掉；
<p>{{ '<em>hello</em>' | striptags }}</p>
列表操作
first：取第一个元素
<p>{{ [1,2,3,4,5,6] | first }}</p>
last：取最后一个元素
<p>{{ [1,2,3,4,5,6] | last }}</p>
length：获取列表长度
<p>{{ [1,2,3,4,5,6] | length }}</p>
sum：列表求和
<p>{{ [1,2,3,4,5,6] | sum }}</p>
sort：列表排序
<p>{{ [6,2,3,1,5,4] | sort }}</p>

flask-wtf使用
https://www.jianshu.com/p/3fd2a1f155f3
