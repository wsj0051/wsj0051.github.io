---
title: Termux使用记录
date: 2020-03-05 17:29:33
tags: "termux"
top_img: https://cdn.jsdelivr.net/gh/wsj0051/IMG/blog/20200305173536.jpg
cover: https://cdn.jsdelivr.net/gh/wsj0051/IMG/blog/15251875958364.jpg
---

# Termux使用记录

> 参考原文链接：https://www.sqlsec.com/2018/05/termux.html

## 创建软链（直接跳转到手机内存卡目录的超链）

```
ln -s /data/data/com.termux/files/home/storage/shared/wsj0051 wsj0051
```

## 修改启动问候语

```
vim $PREFIX/etc/motd
```

如果没有安装vim的话会有提示，跟据提示安装：pkg install vim
### 修改启动语为sh脚本方式

```
cd $PREFIX/etc
vim motd
motd内容修改为： 
#!$PREFIX/bin/bash
neofetch
修改后保存并退出，执行以下命令
mv motd profile.d/motd.sh
```
### 如果启动后出现触发两次，将sh文件执行语句放进.zshrc下
```
mv $PREFIX/etc/profile.d/motd.sh .
echo "$PREFIX/bin/bash ~/motd.sh" >> ~/.zshrc

```



### 修改neofetch配置
```
cd .config/neofetch
vim config.conf

```
 `可以修改展示的信息，颜色，修改ascii_distro="linux"将默认的安卓换为linux ` 

## 手机已经root

>安装tsu,这是一个su的termux版本,用来在termux上替代su:

```
pkg install tsu
```

## 搭建Hexo 博客

```
pkg install nodejs
pkg install git
npm install hexo-cli -g
mkdir hexoblog
cd hexoblog
hexo init
```

> 更多资料参考官方api:https://hexo.io/zh-cn/docs/index.html

## 也可以使用一键脚本

![20191104004.jpg](https://cdn.jsdelivr.net/gh/wsj0051/IMG/blog/20200315001.jpg)

一键脚本直接用别人写好的，github或gitee有很多，直接在Termux内输入以下命令安装本工具：

```
pkg i -y wget && bash -c "$(wget -qO- 'https://github.com/st1020/Stone-Termux-Tool/raw/master/st.sh')"
```

或者：

```
pkg i -y wget && bash -c "$(wget -qO- 'https://gitee.com/st1020/Stone-Termux-Tool/raw/master/st.sh')"
```

## 使用atilo安装linux

在Termux安装Linux的bash脚本

```
echo "deb [trusted=yes] https://yadominjinta.github.io/files/ termux extras" >> $PREFIX/etc/apt/sources.list
pkg in atilo-cn
```

> Atilo           2.0
> Usage: atilo [命令] [参数]
>
> Atilo 是一个用来帮助你在termux上安装不同的GNU/Linux发行版的程序
>
> 命令:
> list             列出可用镜像
> images           移除本地的镜像
> pull             拉取远的镜像
> run              运行镜像
> help             帮助

>安装linux之后就可以配置java环境变量，安装tomcat了。

{% dplayer "url=http://wsj0051.gitee.io/file/termux_hexo.mp4" "theme=#FADFA3" "autoplay=false" %}