# git

## 配置git
### 配置全局用户名邮箱信息

```shell
git config --global user.name  "your user name"
git config --global user.email "your email"
```

### 让Git显示颜色

```shell
git config --global color.ui true
```

### git设置别名

```shell
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
### clon代码两种方式

1. HTTPS的格式为：`https://github.com/用户名/仓库名.git`
2. SSH的格式为：`git@github.com:用户名/仓库名.git`

git图形化工具[sourcetree](https://www.sourcetreeapp.com/)

## SSH
### ssh生成密钥

```shell
ssh-keygen -t rsa -C "your email"
```

一个密钥同时在github和gitee使用，修改`.ssh`目录下config文件

```text
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

```shell
ssh -T git@github.com
```

### SSH协议代理设置

修改 `ssh` 配置文件 `~/.ssh/config`
`Windows ssh` 配置文件路径：`C:\Users\你的用户名\.ssh\config`
`Linux ssh` 配置文件路径：`/home/你的用户名/.ssh/config`
```
# 全局 -S 代表走socks代理。
//对于使用git@协议的，可以配置socks5代理
//在~/.ssh/config 文件后面添加几行，没有可以新建一个
//socks5
# 只为特定域名设定
Host github.com
User git
ProxyCommand connect -S 127.0.0.1:10808 %h %p

//http || https
Host github.com
User git
ProxyCommand connect -H 127.0.0.1:1080 %h %p
```

## git代理
### 设置全局代理
```
//http
git config --global https.proxy http://127.0.0.1:10808
//https
git config --global https.proxy https://127.0.0.1:10808
//使用socks5代理的 例如ss，ssr 1080是windows下ss的默认代理端口,mac下不同，或者有自定义的，根据自己的改
git config --global http.proxy socks5://127.0.0.1:10808
git config --global https.proxy socks5://127.0.0.1:10808
```
### 取消全局代理
```
git config --global --unset http.proxy
git config --global --unset https.proxy
```
### 只对github.com使用代理，其他仓库不走代理
```
git config --global http.https://github.com.proxy socks5://127.0.0.1:10808
git config --global https.https://github.com.proxy socks5://127.0.0.1:10808
```
### 取消github代理
```
git config --global --unset http.https://github.com.proxy
git config --global --unset https.https://github.com.proxy
```
