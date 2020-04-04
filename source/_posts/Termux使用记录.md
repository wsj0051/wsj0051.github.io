---
title: Termux使用记录
date: 2020-03-05 17:29:33
tags: "Termux"
categories: "学习"
top_img: https://cdn.jsdelivr.net/gh/wsj0051/IMG/blog/20200305173536.jpg
cover: https://cdn.jsdelivr.net/gh/wsj0051/IMG/blog/15251875958364.jpg
---

# Termux使用记录
## 访问手机存储
```
termux-setup-storage
```

> 执行上面的命令以后，会跳出一个对话框，询问是否允许 Termux 访问手机存储，点击"允许"。

## 创建软链

> 直接跳转到手机内存卡对应目录的快捷方式

```
ln -s /data/data/com.termux/files/home/storage/shared/wsj0051 wsj0051
```

## 修改为清华源

编辑源文件

```
apt edit-sources
```

将原来的`https://termux.net`官方源替换为`http://mirrors.tuna.tsinghua.edu.cn/termux`

```
# The termux repository mirror from TUNA:
deb https://mirrors.tuna.tsinghua.edu.cn/termux stable main
```

如果清华源 出一些问题的话，大家可以尝试先用着官方源：

```
# The main termux repository:
deb https://termux.org/packages/ stable main
```

## 安装基本工具

```
pkg update
pkg upgrade
pkg install vim curl wget git unzip unrar
```

## 修改终端配色

```
sh -c "$(curl -fsSL https://github.com/Cabbagec/termux-ohmyzsh/raw/master/install.sh)"  
```

> 脚本运行后会提示选择背景色和字体

```
Enter a number, leave blank to not to change: 14
Enter a number, leave blank to not to change: 6
```

> 如果需要重新修改配色

```
~/termux-ohmyzsh/install.sh
```

### Termux快捷按键

> 执行以下命令后退出，重新启动

```
echo "extra-keys = [['ESC','/','-','HOME','UP','END','PGUP'],['TAB','CTRL','ALT','LEFT','DOWN','RIGHT','PGDN']]" >> $HOME/.termux/termux.properties
```

termux快捷键：
echo "extra-keys = [['ESC','/','-','HOME','UP','END','PGUP'],['TAB','CTRL','ALT','LEFT','DOWN','RIGHT','PGDN']]" >> $HOME/.termux/termux.properties

## 修改启动问候语

```
vim $PREFIX/etc/motd
```

> 如果没有安装vim的话会有提示，跟据提示安装：pkg install vim

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

## git配置全局用户名邮箱信息

```
git config --global user.name  "your user name"
git config --global user.email "your email"
```

## git生成密钥

```
git ssh-keygen -t rsa
```

> 将公钥配置进github，gitee，gitlab等代码仓库

### 验证公钥是否绑定成功

```
ssh -T git@github.com
```

> GitHub在clone仓库时，有两种URL可以选择，分别为HTTPS和SSH：
>
> HTTPS的格式为：https://github.com/用户名/仓库名.git
>
> SSH的格式为：git@github.com:用户名/仓库名.git
>
> 在配置完公钥后，选择使用 **SSH** 的URL来clone仓库，在Push代码到GitHub时，可以免输入用户名、密码。

## npm安装http-server
```
npm install -g http-server
```
然后，运行 Server。

```
http-server
```
> 正常情况下，命令行会提示 Server 已经在 8080 端口运行了，并且还会提示外部可以访问的 IP 地址。

## 使用ecj termux-tools dx编译java文件

1. 更新资源

   ```
   pkg update & pkg upgrade
   ```

2. 安装所需软件

   ```
   pkg install ecj termux-tools dx
   ```

3. 创建java文件Hello.java

   ```java
   public class Hello{
       public static void main(String[] args){
        System.out.println("Hello World!");
       }
   }
   ```

4. 编译java文件

   ```
   ecj Hello.java
   ```

5. 生成安卓虚拟机文件

   ```
   dx --dex --output=Hello.dex Hello.class
   ```

6. 安卓虚拟机运行程序

   ```
   dalvikvm -cp Hello.dex Hello
   ```

7. 更简单方式，创建shell脚本

   ```
   vim ecj.sh
   ```

   ```
   #!/usr/bin/sh
   ecj "$1.java"
   dx --dex --output="$1.dex" "$1.class"
   dalvikvm -cp "$1.dex" "$1"
   ```

8. 执行shell编译java

   ```
   sh ecj.sh Hello
   ```

## 使用一键脚本配置termux

![20191104004.jpg](https://cdn.jsdelivr.net/gh/wsj0051/IMG/blog/20200315001.jpg)

一键脚本直接用别人写好的，github或gitee有很多，直接在Termux内输入以下命令安装本工具：

```
pkg i -y wget && bash -c "$(wget -qO- 'https://github.com/st1020/Stone-Termux-Tool/raw/master/st.sh')"
```

或者：

```
pkg i -y wget && bash -c "$(wget -qO- 'https://gitee.com/st1020/Stone-Termux-Tool/raw/master/st.sh')"
```

### 手机通知栏时间打开时分秒

手机通知栏的时间没有精确到秒，手机root后可以打开

1. 使用一键方式安装adb工具

2. 执行以下命令：

   ```
   adb shell 
   settings put secure clock_seconds 1
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


```
JAVA_HOME=/usr/local/java/jdk1.8.0_241
PATH=$JAVA_HOME/bin:$PATH
CLASSPATH=$JAVA_HOME/jre/lib/ext:$JAVA_HOME/lib/tools.jar
export PATH JAVA_HOME CLASSPATH
```


> 将以上内容拷进文件末尾，退出后执行以下命令让配置生效

~~~
 source /etc/profile
~~~

> 使用java -version查看环境变量是否配置成功,成功后就可以启动tomcat了

{% dplayer "url=http://wsj0051.gitee.io/file/video/termux.mp4" "theme=#FADFA3" "autoplay=false" %}

> 参考原文链接：[国光-Termux](https://www.sqlsec.com/2018/05/termux.html) [简书](https://www.jianshu.com/p/cedba5bdc466?utm_campaign)