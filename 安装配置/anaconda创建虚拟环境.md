# conda 安装虚拟环境
~~~
conda create -n my_env_name python=3.6
~~~
`my_envs_name`是创建虚拟环境的名字 `python=3.6`是你的虚拟环境是基于python3.6的

激活虚拟环境
~~~
conda/source activate my_env_name
~~~

退出虚拟环境
~~~
conda/source deactivate
~~~

# 查看Conde环境下所有的虚拟环境
~~~
conda info --envs
~~~

# 删除Conda虚拟环境
~~~
conda remove -n my_env_name --all
~~~