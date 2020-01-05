# 1. 本地配置
开始配置, 使用以下命令即可:
```bash
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```
注意: 
git config命令的–global参数，用了这个参数，表示你这台机器上所有的 Git 仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和 Email 地址(如需要, 可自行搜索)。
使用git config --list可查看配置的相关信息, 出现以下信息, 则表明成功:

![](https://raw.githubusercontent.com/krislinzhao/IMGclound/master/img/20200105112228.png)

# 2. 连接Github或者码云
- 1.生成ssh公钥:
```bash
ssh-keygen -t rsa -C "email@example.com"  
```
三次回车即可生成 ssh key, 这里的邮箱最好填和刚才一样的
然后用文本编辑器(如notepad)打开id_rsa.pub这个文件, 全选复制.
![](https://raw.githubusercontent.com/krislinzhao/IMGclound/master/img/20200105112515.png)

- 2.接下来到GitHub上，打开“Account settings”--“SSH Keys”页面，然后点“Add SSH Key”，填上Title（随意写），在Key文本框里粘贴 id_rsa.pub文件里的全部内容。
![](https://raw.githubusercontent.com/krislinzhao/IMGclound/master/img/20200105112819.png)

点“Add Key”，你就应该看到已经添加的Key，可以添加多个Key
![](https://raw.githubusercontent.com/krislinzhao/IMGclound/master/img/20200105112919.png)

- 3.验证是否成功，在git bash里输入下面的命令
```bash
ssh -T git@github.com
```
如果初次设置的话，会出现如下界面，输入yes 同意即可
![](https://raw.githubusercontent.com/krislinzhao/IMGclound/master/img/20200105113115.png)