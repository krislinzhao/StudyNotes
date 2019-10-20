flask  应用部署目录
|——app                    应用目录
|  |__ static             静态文件价(放CSS和JavaScript文件主要是用来渲染前端页面)
|  |__ templates          模板(放html文件，写页面的)
|  |       |__base.html             基础模板，后面的一些模板会继承它
|  |       |__edit_profile.html     编辑个人信息的页面
|  |       |__index.html            根路由下的页面，欢迎界面
|  |       |__login.html            登录页面
|  |       |__register.html         注册页面
|  |       |__user.html             用户页面
|  |__ __init__.py        初始化 
|  |__ forms.py           表单
|  |__ models.py          数据库中的表
|  |__ routes.py          视图函数
|——migrations             数据库迁移文件
|——config.py              配置文件
|——myblog.py              启动文件
|__requirements.txt       环境文件,项目用到的包和版本
         