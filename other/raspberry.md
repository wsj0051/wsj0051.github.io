# 树莓派使用记录

## 系统安装
### 校验包，解压

```shell
sha1sum 2013-09-25-wheezy-raspbian.zip
unzip  2013-09-25-wheezy-raspbian.zip
```

查看当前哪些设备已经挂载,`df -h`，插入u盘或sd卡再执行一次
为了防止在写入镜像的时候有其他读取或写入，我们需要卸载设备。两个分区都要卸载。

```shell
umount  /dev/sdb1
umount  /dev/sdb2
```

### xz烧录命令(该命令尝试失败)

```shell
sudo xz -cd kali-2017.3-rpi3-nexmon.img.xz> /dev/sdb
```

查看烧录进度`sudo pkill -USR1 -n -x xz`

### img格式镜像烧录命令如下（亲测成功）

```shell
sudo dd bs=4M if=2013-09-25-wheezy-raspbian.img of=/dev/sdb
```

查看烧录进度`sudo pkill -USR1 -n -x dd`

### 连接wifi
```
## To use this file, you should run command "systemctl disable network-manager" and reboot system. (Do not uncomment this line!) ##

#country=CN
#ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
#update_config=1


## WIFI 1 (Do not uncomment this line!)

network={
    ssid="coolxiaomi"
    psk="coolxiaomi"
    priority=1
    id_str="wifi-1"
}


## WIFI 2 (Do not uncomment this line!)

network={
    ssid="wsj0051"
    psk="752838157w"
    priority=2
    id_str="wifi-2"
}


```

### 卸载软件

卸载但不删除配置

```shell
apt-get remove packagename
```

卸载并删除配置

```shell
apt-get purge packagename
```

### 基础命令

安装软件 `apt-get install softname1 softname2 softname3……`

卸载软件 `apt-get remove softname1 softname2 softname3……`

卸载并清除配置 `apt-get remove –purge softname1`

更新软件信息数据库 `apt-get update`

进行系统升级 `apt-get upgrade`

搜索软件包 `apt-cache search softname1 softname2 softname3……`

安装deb软件包 `dpkg -i xxx.deb`

删除软件包 `dpkg -r xxx.deb`

连同配置文件一起删除 `dpkg -r –purge xxx.deb`

查看软件包信息 `dpkg -info xxx.deb`

查看文件拷贝详情 `dpkg -L xxx.deb`

查看系统中已安装软件包信息 `dpkg -l`

重新配置软件包 `dpkg-reconfigure xxx`

清除所有已删除包的残馀配置文件

```shell
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
```

> dpkg安裝的可以用apt卸載，反之亦可。

### aptitude

`aptitude update` 更新可用的包列表

`aptitude upgrade` 升级可用的包

`aptitude dist-upgrade` 将系统升级到新的发行版

`aptitude install pkgname` 安装包

`aptitude remove pkgname` 删除包

`aptitude purge pkgname` 删除包及其配置文件

`aptitude search string` 搜索包

`aptitude show pkgname` 显示包的详细信息

`aptitude clean` 删除下载的包文件

`aptitude autoclean` 仅删除过期的包文件


## 基地64位系统
### 程序快捷目录位置
```
/usr/share/applications
```
### vim不能右键粘贴

1. 编辑 vim 的默认配置文件
    ```
    vim /usr/share/vim/vim80/defaults.vim
    ```
2. 转至第 70 行，找到：
    ```
    if has('mouse')

    set mouse=a

    endif
    ```
3. 将 `set mouse=a` 改为：`set mouse-=a`

4. 输入 `:wq` 保存即可生效。
   
### 启用CecOS-CaaS容器云

    ```
    开机自动启动 CecOS CaaS容器云 服务
    systemctl enable cecos-caas.service

    启动 CecOS CaaS容器云 服务
    systemctl start cecos-caas.service

    ######

    停止 CecOS CaaS容器云 服务
    systemctl stop cecos-caas.service

    禁止 CecOS CaaS容器云 服务开机启动
    systemctl disable cecos-caas.service
    ```

### 桌面版系统单声道改为立体声输出
    编辑文件 /usr/share/pulseaudio/alsa-mixer/profile-sets/default.conf

    在下面的内容每行最前面添加 ";" 符号，以注释掉以下内容：
    ```
    [Mapping analog-mono]
    device-strings = hw:%f
    channel-map = mono
    paths-output = analog-output analog-output-lineout analog-output-speaker analog-output-headphones analog-output-headphones-2 analog-output-mono
    paths-input = analog-input-front-mic analog-input-rear-mic analog-input-internal-mic analog-input-dock-mic analog-input analog-input-mic analog-input-linein analog-input-aux analog-input-video analog-input-tvtuner analog-input-fm analog-input-mic-line analog-input-headset-mic
    priority = 7
    ```
    下面是注释后的内容：
    ```
    ;[Mapping analog-mono]
    ;device-strings = hw:%f
    ;channel-map = mono
    ;paths-output = analog-output analog-output-lineout analog-output-speaker analog-output-headphones analog-output-headphones-2 analog-output-mono
    ;paths-input = analog-input-front-mic analog-input-rear-mic analog-input-internal-mic analog-input-dock-mic analog-input analog-input-mic analog-input-linein analog-input-aux analog-input-video analog-input-tvtuner analog-input-fm analog-input-mic-line analog-input-headset-mic
    ;priority = 7
    ```
    最后重启系统生效。

## 安装软件
### 安装chromium

[下载地址](http://ports.ubuntu.com/pool/universe/c/chromium-browser/?C=M;O=D)
使用`sudo dpkg -i *.deb`安装，按顺序安装

1. chromium-codecs-ffmpeg-extra
2. chromium-browser
3. chromium-chromedriver


### 安装smb
```
sudo apt-get install samba 
sudo apt-get install samba-common

```
### smb共享

接下来修改 Samba 的配置文件，打开/etc/samba/smb.conf，在末尾添加以下内容：
```
[share] # 共享名称
comment = my share disk # 描述信息
path = /home/pi/share # 共享的目录
browserable = yes # 访问权限
writable = yes # 写入权限
```
smb重启
```
sudo service smbd restart 
```
挂载硬盘
```
sudo df -h
sudo mount /dev/sda1 /home/pi/share
```
设置硬盘的开机挂载
```
sudo vi /etc/fstab 
/dev/sda1 /home/pi/share ext4 defaults 0 0

```

### mysql安装

1. 安装mysql

    ```shell
    sudo apt install mysql-server
    sudo usermod -d /var/lib/mysql/ mysql
    ```

2. 停止、启动命令

    ```shell
    sudo service mysql stop
    sudo service mysql start
    ```

3. 连接命令

    ```shell
    mysql -u root -p
    ```


## docker 相关

### 启用和运行Docker服务
默认没有启用 Docker服务，需要手动启动。

开机自动启动Docker服务
```
systemctl enable docker.service
```
启动Docker服务
```
systemctl start docker.service
```

停止Docker服务
```
systemctl stop docker.service
```
禁止Docker服务开机启动
```
systemctl disable docker.service
```
打开树莓派终端，输入下面命令获得超级用户权限
```shell
sudo -i
```

### 安装Alist
```
curl -fsSL "https://alist.nn.ci/v3.sh" | bash -s install
```

### php
```
docker run -d --restart unless-stopped --privileged=true -p 5678:80 --name php-env youshandefeiyang/php-env:arm64
docker cp /home/pi/公共/yy.php php-env:/var/www/html/

```

### xteve
```
docker run -d \
  --restart unless-stopped \
  --name=xteve_guide2go \
  --net=host \
  --log-opt max-size=10m \
  --log-opt max-file=3 \
  -e TZ="Asia/Shanghai" \
  -v /home/pi/appdata/xteve/:/root/.xteve:rw \
  -v /home/pi/appdata/xteve/_config/:/config:rw \
  -v /home/pi/appdata/xteve/_guide2go/:/guide2go:rw \
  -v /tmp/xteve/:/tmp/xteve:rw \
  -v /home/pi/appdata/tvheadend/data/:/TVH \
  jjm2473/xteve_guide2go

 
```

### aria2
```
docker run -d \
    --name aria2-pro \
    --restart unless-stopped \
    --log-opt max-size=1m \
    -e PUID=$UID \
    -e PGID=$GID \
    -e UMASK_SET=022 \
    -e RPC_SECRET=wsj0051 \
    -e RPC_PORT=6800 \
    -p 6800:6800 \
    -e LISTEN_PORT=6888 \
    -p 6888:6888 \
    -p 6888:6888/udp \
    -v $PWD/appdata/aria2/aria2-config:/config \
    -v $PWD/downloads:/downloads \
    p3terx/aria2-pro
```
### aria2 webUi控制台
```
docker run -d \
    --name ariang \
    --log-opt max-size=1m \
    --restart unless-stopped \
    -p 6880:6880 \
    p3terx/ariang
```

### mysql
+ 安装
     ```shell
     sudo    docker run --name mysql \
     -v /home/pi/mysql:/var/lib/mysql \
     -e MYSQL_ROOT_PASSWORD=pi \
     -e MYSQL_DATABASE=nextcloud \
     -e MYSQL_USER=nextcloud \
     -e MYSQL_PASSWORD=password \
     -p 3306:3306 \
     --restart=always \
     -d mysql/mysql-server
     ```
     
+ 赋予本地文件夹读写权限
     ```
     sudo chmod -R 777 /home/mysql
     ```

+ 进入mysql容器，并登陆mysql
     ```
     docker exec -it mysql bash
     mysql -u root -p
     ```
+ 创建用户，赋予权限
     ```
     Create user 'nextcloud'@'%' identified by '123456';
     grant all privileges on nextcloud.* to 'nextcloud'@'%';
     ```
+ 开启远程访问权限
     ```
     use mysql;
     select host,user from user;
     alter user 'root'@'%' identified with mysql_native_password by 'pi'; 
     flush privileges;
     ```
 + 开机启动
     ```
     sudo docker update mysql --restart=always

     ```


### 安装owncloud

1. 在docker中安装私有云，在树莓派终端输入下面命令。（注意：为了避免出错，下面用到的所有命令以及要修改的内容也给大家提供了文档版本，可以在树莓派爱好者基地微信公众号发送【私有云安装】获得）

    ```shell
    docker run -d -p 8888:80  --name nextcloud -v /data/nextcloud/:/var/www/html/ --restart=always   --privileged=true  arm64v8/nextcloud
    ```

2. 修改配置文件


    ```shell
    mkdir /data/nextcloud/config
    sudo docker cp nextcloud:/var/www/html/config/config.sample.php  /data/nextcloud/config/config.sample.php
    sudo docker cp /data/nextcloud/config/config.php  nextcloud:/var/www/html/config/config.php
    nano /data/nextcloud/config/config.sample.php
    ```

    修改`trusted_domains`里的值

    ```txt
    1 => preg_match('/cli/i',php_sapi_name())?'127.0.0.1':$_SERVER['SERVER_NAME']
    ```

3. 安装nextcloud客户端

    ```shell
    apt install nextcloud-desktop-l10n nextcloud-desktop -y
    ```
