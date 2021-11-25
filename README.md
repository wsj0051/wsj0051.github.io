# gitbook

## 说明
- gitbook源码，修改后自动同步到[gitbook](https://wsj0051.gitbok.io)
- actions自动打包并发布至[gitee](https://wsj0051.gitee.io)

## 安装

+ nvm安装
  - 从github查看[nvm仓库](https://github.com/nvm-sh/nvm)

    ```
    cd ~/
    git clone https://github.com/nvm-sh/nvm.git .nvm
    cd ~/.nvm
    . ./nvm.sh
    ```
  - 修改文件` ~/.bashrc, ~/.profile, or ~/.zshrc`末尾配置环境变量

    ```
    export NVM_DIR="$HOME/.nvm"
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
    [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

    ```
  - window下安装[nvm-windows](https://hub.fastgit.org/coreybutler/nvm-windows) 
+ 安装nodejs,使用nodejs v10.23.0 版本
    ```
    nvm install v10.23.0
    ```
+ gitbook安装
    ```
    npm install gitbook-cli -g
    gitbook -V
    ```
+ 速度慢时安装cnpm,使用cnpm安装，命令中npm替换为cnpm即可
    ```
    npm install -g cnpm -registry=https://registry.npm.taobao.org
    ```
+ 安装[ebook-convert](https://calibre-ebook.com/download_linux)，查看是否安装成功[^1]
  ```
  ebook-convert --version
  ```
+ 常用命令：
  - `gitbook pdf` 生成pdf
  - `gitbook -h` 帮助
  - `gitbook install` 进行插件安装
  - `gitbook build & gitbook serve` 构建并启动服务
  - `gitbook fetch [version]` 获取[版本]下载并安装<版本>
  - `gitbook ls-remote` 列出NPM上的可用版本：

## 卸载
找到`C:\Users\{User}\.gitbook`并删除此文件夹
```
npm uninstall -g gitbook
npm uninstall -g gitbook-cli
```
清除npm缓存
```
npm cache clean -f
```

[^1]: 下载4.5.0版本可以生成pdf不报错
