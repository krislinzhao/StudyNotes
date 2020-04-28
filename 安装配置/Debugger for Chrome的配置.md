- version : 你定义这个配置文件的版本，生成的时候默认是0.2.0
- configuration：配置域
- name：配置文件的名字，可以自己起
- type：调试的类型，node是vscode本身就支持，其他就需要下载插件了
- request : 配置文件的请求类型，有launch和attach两种，launch是需要服务器的需要配置url，这里我就用的它，attach就比较麻烦了，因为配置launch也能用，所以我就没有配置attach了
- url:这个是chrome插件带的，指定访问的链接，到这里我觉得就个缺点了，url只能配置死链接，就算用预定义变量也不能做到多项目自动识别要打开的HTML，可能是我没有发现其他的预定义变量，如果有大神知道，欢迎在评论里留言
- webRoot：也是chrome插件带的，指定根目录或者执行文件
- ${workspaceRoot}:就是你打开vscode读取的项目目录
- sourceMaps:默认是启用的，对于打包的调试，大神们就必须开启了
- userDataDir：临时目录,专门保存调试过程产生的东西，这个字段是为了重新打开一个浏览器窗口，不会强制关闭已经打开的浏览器
launch.json中有很多属性可以设置, 通过智能提示查看有那些属性可以设置, 如果要查看属性的具体含义, 可以把鼠标悬停在属性上面, 会属性的使用说明.

在launch.json中会使用到一些预定变量, 这些变量的具体含义如下：

- ${workspaceRoot}：VSCode中打开文件夹的路径
- ${workspaceRootFolderName}：VSCode中打开文件夹的路径, 但不包含"/"
- ${file} ：当前打开的文件
- ${relativeFile}：当前打开的文件,相对于workspaceRoot
- ${fileBasename} ：当前打开文件的文件名, 不含扩展名
- ${fileDirname} ：当前打开文件的目录名
- ${fileExtname}：当前打开文件的扩展名
- ${cwd} ：当前启动时的工作目录
