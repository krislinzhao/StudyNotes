lin-cms-flask //项目
|───app         // app目录
|    │───app.py // 创建Flask app及应用扩展
|    │───__init__.py // 默认的包初始化文件
|    │
|    │───api // api目录
|    │    │———__init__.py // 默认的包初始化文件
|    │    │
|    │    ├───cms // 开发CMS API目录
|    │    │   │__init__.py // 创建CMS蓝图
|    │    │
|    │    ├───v1 // 开发普通API目录
|    │        │__init__.py // 创建v1蓝图
|    │
|    ├───config // 配置文件目录
|    │     │ secure.py // 有关于安全的配置
|    │     │ setting.py // 基础配置
|    |     | log.py     // 系统日志配置
|    │
|    ├───libs // 类库文件夹
|    │     │ error_code.py // 自定义异常文件
|    │     │ utils.py // 工具文件
|    │
|    ├───models // 模型文件夹
|    │     │ book.py // book模型文件
|    │     │ __init__.py // 默认的包初始化文件
|    │
|    ├───plugins // 插件目录
|    │      
|    │
|    ├───validators // 校验类存放目录
|           │   forms.py // 校验类文件
|           │   __init__.py // 默认的包初始化文件
|───logs    //生成的日志文件
|───tests   //测试文件
|───add_super.py //添加一个超级管理员
│───code.md // 记录自定义的返回码和信息
│───fake.py // 测试和做假数据的脚本
|───README.MD //说明文件
|───requirements.txt //依赖包文件
|───starter.py // 程序的开始文件
