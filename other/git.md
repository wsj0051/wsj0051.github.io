# git
配置全局用户名邮箱信息
```
git config --global user.name  "your user name"
git config --global user.email "your email"
```
让Git显示颜色
```
git config --global color.ui true
```
git设置别名
```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
Git在clone仓库时，有两种URL可以选择，分别为HTTPS和SSH：

1. HTTPS的格式为：`https://github.com/用户名/仓库名.git`
2. SSH的格式为：`git@github.com:用户名/仓库名.git`

git图形化工具[sourcetree](https://www.sourcetreeapp.com/)

## ssh生成密钥
```
ssh-keygen -t rsa -C "your email"
```
一个密钥同时在github和gitee使用，修改`.ssh`目录下config文件
```
# gitee
Host gitee.com
HostName gitee.com
HostkeyAlgorithms +ssh-rsa 
PubkeyAcceptedAlgorithms +ssh-rsa
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
```
将公钥配置到github后,验证公钥是否绑定成功
```
ssh -T git@github.com
```
