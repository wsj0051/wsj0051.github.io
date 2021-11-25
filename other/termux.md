# Termux

## 访问手机存储
```
termux-setup-storage
```

执行上面的命令以后，会跳出一个对话框，询问是否允许 Termux 访问手机存储，点击"允许"。
## 创建软链

直接跳转到手机内存卡对应目录的快捷方式
```
ln -s /data/data/com.termux/files/home/storage/shared/wsj0051 wsj0051
```

## 修改为清华源
使用如下命令行替换官方源为 [TUNA 镜像源](https://mirror.tuna.tsinghua.edu.cn/help/termux/)
```
sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24 stable main@' $PREFIX/etc/apt/sources.list
sed -i 's@^\(deb.*games stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/game-packages-24 games stable@' $PREFIX/etc/apt/sources.list.d/game.list
sed -i 's@^\(deb.*science stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/science-packages-24 science stable@' $PREFIX/etc/apt/sources.list.d/science.list
apt update && apt upgrade
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

脚本运行后会提示选择背景色和字体

```
Enter a number, leave blank to not to change: 14
Enter a number, leave blank to not to change: 6
```

 如果需要重新修改配色

```
~/termux-ohmyzsh/install.sh
```
## Termux快捷按键
```
mkdir $HOME/.termux;
echo "extra-keys = [ \
    ['ESC','<','>','BACKSLASH','=','^','$','()','{}','[]','ENTER'], \
    ['TAB','&',';','/','~','%','*','HOME','UP','END','PGUP'], \
    ['CTRL','FN','ALT','|','-','+','QUOTE','LEFT','DOWN','RIGHT','PGDN'] \
    ]
" >> $HOME/.termux/termux.properties
```

## 修改启动问候语
```
vim $PREFIX/etc/motd
```
如果没有安装vim的话会有提示，根据提示安装：`pkg install vim`

修改启动语为sh脚本方式
```
cd $PREFIX/etc
vim motd
```
motd内容修改为脚本文件，内容为： 
```
#!$PREFIX/bin/bash
neofetch
```

修改后保存并退出，执行以下命令

```
mv motd profile.d/motd.sh
```
### 重复执行问题

如果启动后出现触发两次，将sh文件执行语句放进.zshrc下

```
mv $PREFIX/etc/profile.d/motd.sh .
echo "$PREFIX/bin/bash ~/motd.sh" >> ~/.zshrc
```

### 修改neofetch配置
```
cd .config/neofetch
vim config.conf
```
可以修改展示的信息，颜色，修改`ascii_distro="linux"`将默认的安卓换为linux 

## npm安装http-server
```
npm install -g http-server
```
然后，运行 Server。
```
http-server
```
正常情况下，命令行会提示 Server 已经在 8080 端口运行了，并且还会提示外部可以访问的 IP 地址。

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
   ```
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
7. 更简单方式，创建shell脚本`vim ecj.sh`
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

## 手机通知栏时间打开时分秒
手机通知栏的时间没有精确到秒，手机root后可以打开

1. 使用一键方式安装adb工具
2. 执行以下命令：
   ```
   pkg install tsu
   adb shell 
   settings put secure clock_seconds 1
   ```

## 使用atilo安装linux
1. 在Termux安装Linux的bash脚本
	```
	echo "deb [trusted=yes] https://yadominjinta.github.io/files/ termux extras" >> $PREFIX/etc/apt/sources.list
	pkg in atilo-cn
	```
2. 安装debian
	```
	atilo pull debian
	```
3. 运行debian
	```
	atilo run debian
	```

### 配置apache java环境
1. 在usr/local下创建java文件夹，将下载好的tar文件移动到java目录下，对应手机目录为`/data/data/com.termux/files/home/.atilo/debian/usr/local/java`  
2. 解压jdk和tomcat
    ```
    cd /usr/local/java
    tar xzf jdk-8u241-linux-arm64-vfp-hflt.tar.gz
    tar xzf apache-tomcat-9.0.31.tar.gz
    ```
3. 以下内容拷进`/etc/profile`文件末尾
    ```
    JAVA_HOME=/usr/local/java/jdk1.8.0_241
    PATH=$JAVA_HOME/bin:$PATH
    CLASSPATH=$JAVA_HOME/jre/lib/ext:$JAVA_HOME/lib/tools.jar
    export PATH JAVA_HOME CLASSPATH
    ```
4. 保存退出后执行以下命令让配置生效，使用`java -version`查看环境变量是否配置成功
    ```
     source /etc/profile
    ```

## 一键安装tmoe-linux 
```
   . <(curl -L gitee.com/mo2/linux/raw/2/2)
```


## 参考链接
1. [国光-Termux](https://www.sqlsec.com/2018/05/termux.html) 
2. [简书](https://www.jianshu.com/p/cedba5bdc466?utm_campaign)
3. [tmoe](https://github.com/2moe/tmoe-linux)
