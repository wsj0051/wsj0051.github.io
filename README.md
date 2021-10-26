# 使用gitbook生成

## 说明
- gitbook源码，修改后自动同步到[gitbook](https://wsj0051.gitbok.io)
- actions自动打包并发布至[gitee](https://wsj0051.gitee.io)

## Nodejs安装
使用nodejs v10.23.0 版本，从github查看[nvm仓库](https://github.com/nvm-sh/nvm)

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
速度慢时安装cnpm,使用cnpm安装，命令中npm替换为cnpm即可
```
npm install -g cnpm -registry=https://registry.npm.taobao.org
```
## 安装ebook-convert
```
sudo apt install calibre
```
[calibre - Download calibre (calibre-ebook.com)](https://calibre-ebook.com/download)

其他命令：
`gitbook pdf`[^1] 生成pdf
`gitbook -h` 帮助

`gitbook install` 进行插件安装

`gitbook build & gitbook serve` 构建并启动服务

[^1]: 使用pdf命令如果报错**Assertion failed: new_time >= loop->time, file c:\ws\deps\uv\src\win\core.c, line 309**，使用新版本替换安装https://github.com/nodejs/help/files/4831517/node.zip
