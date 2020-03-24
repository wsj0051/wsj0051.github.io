---
title: Termux使用记录
date: 2020-03-05 17:29:33
tags: "Termux"
categories: "学习"
top_img: https://cdn.jsdelivr.net/gh/wsj0051/IMG/blog/20200305173536.jpg
cover: https://cdn.jsdelivr.net/gh/wsj0051/IMG/blog/15251875958364.jpg
---

# Termux使用记录

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
```
motd内容修改为脚本文件，内容为： 

> #!$PREFIX/bin/bash
> neofetch

修改后保存并退出，执行以下命令

```
mv motd profile.d/motd.sh
```

### 重复执行问题

> 如果启动后出现触发两次，将sh文件执行语句放进.zshrc下

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

### 安装debian

```linux
atilo pull debian
```

### 运行debian

```
atilo run debian
```

### debian对应手机路径

> /data/data/com.termux/files/home/.atilo/debian

### 解压tar文件

> 在usr/local下创建java文件夹，将下载好的tar文件移动到java目录下,解压jdk和tomcat

~~~
cd /usr/local/java
tar xzf jdk-8u241-linux-arm64-vfp-hflt.tar.gz
tar xzf apache-tomcat-9.0.31.tar.gz
~~~

### 配置java环境变量

~~~
 vim /etc/profile
~~~

> JAVA_HOME=/usr/local/java/jdk1.8.0_241
> PATH=$JAVA_HOME/bin:$PATH
> CLASSPATH=$JAVA_HOME/jre/lib/ext:$JAVA_HOME/lib/tools.jar
> export PATH JAVA_HOME CLASSPATH

> 将以上内容拷进文件末尾，退出后执行以下命令让配置生效

~~~
 source /etc/profile
~~~

> 使用java -version查看环境变量是否配置成功,成功后就可以启动tomcat了

{% dplayer "url=http://wsj0051.gitee.io/file/video/termux.mp4" "theme=#FADFA3" "autoplay=false" %}

> 参考原文链接：[国光-Termux](https://www.sqlsec.com/2018/05/termux.html)