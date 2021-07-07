# 使用gitbook生成

## 说明
源码在github,actions自动打包并发布至[gitee](https://wsj0051.gitee.io)

## Nodejs安装
最高使用nodejs v10.23.0 版本，从github查看[nvm仓库](https://github.com/nvm-sh/nvm)

```
cd ~/
git clone https://github.com/nvm-sh/nvm.git .nvm
cd ~/.nvm
. ./nvm.sh
```
## 修改环境变量
修改文件` ~/.bashrc, ~/.profile, or ~/.zshrc`末尾配置环境变量

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

```
## 安装nodejs
```
nvm install v10.23.0
```
## gitbook安装
```
npm install gitbook-cli -g
gitbook -V
```
其他命令：

`gitbook -h`帮助

`gitbook install`进行插件安装

`gitbook build & gitbook serve`生成并查看
