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

### ttl连接

准备一个USB转TTL

修改Sd卡中`/boot/config.txt`

在最后一行添加`enable_uart=1`

然后就是连接usb转串口,连接`PIN8->RX`(USB转TTL)，`PIN10->TX`(USB转TTL)，`PIN14-GND`

![树莓派ttl端口](../../assets/images/
raspttl.png)


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
    ssid="OpenWrt"
    psk="********"
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
[基地64位系统](https://gitee.com/openfans-community/Debian-Pi-Aarch64?_from=gitee_search)

### 宝塔虚拟机
#### 启动宝塔虚拟机：
```
./bt_run
```
#### 关闭宝塔虚拟机

为了保证虚拟机的数据同步安全，请按照以下步骤操作：

请ssh登录到虚拟机再执行命令 " init 0 " 关闭虚拟机

关闭后，需要在宝塔虚拟机目录下执行 " ./bt_prog " 命令，检查虚拟机是否已关闭

如果没有任何输出结果，代表虚拟机已正常关闭

如果无法正常关闭虚拟机，请在宝塔虚拟机目录下执行 " ./bt_prog kill " 命令

同样记得再次执行 " ./bt_prog " 命令，检查虚拟机是否已关闭
自动启动

#### 启用开机自动启动
```
./install int
```
#### 取消开机自动启动

```
./install uint
```
默认参数值:

|  项目   | 内容  |
|  ----  | ----  |
| 默认管理端口  | 28888 |
| 默认Web管理用户及密码  | openfans/openfans |
| 宝塔虚拟机ssh端口 | 2222 |
| 宝塔虚拟机root默认密码 | raspberry |


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

接下来修改 `Samba` 的配置文件，打开 `/etc/samba/smb.conf` ，在末尾添加以下内容：
```
[share]
 comment = share for raspbian
 # 映射的路径
 path = /mnt/sda1/
 guest ok = no
 read only = no
 browsable = yes
 create mask = 0777
 directory mask = 0777
 valid users = pi,root
```
smb重启
```
sudo service smbd restart 
```
挂载硬盘
```
sudo df -h
sudo mount /dev/sda1 /usr/local/src/share
```
设置硬盘的开机挂载
```
sudo vi /etc/fstab 
dev/sda1 /mnt/sda1 ext4 defaults,nofail 0 0
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
### 安装Portainer-CE中文版
Portainer-CE是docker的图形化管理面板，提供一个后台页面方便操作docker。

终端输入命令，一键安装Portainer-CE中文版；
```
docker run -d --restart=always --name="portainer" -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data 6053537/portainer-ce:linux-arm64
```


### code-server
```
docker run -d --restart always --name code-server -p 8090:8080 \
  -v /usr/local/src/appdata/code-server/config:/home/coder/.config \
  -v /home/pi/wsj0051:/home/coder/project \
  -u "$(id -u):$(id -g)" \
  -v /home/root/.ssh:/home/root/.ssh \
  -e "DOCKER_USER=$USER" \
  -e PASSWORD=******** \
codercom/code-server:latest
```
### php docker环境
```
docker run -d --restart unless-stopped --privileged=true -p 5678:80 --name php-env youshandefeiyang/php-env:arm64
docker cp /usr/local/src/公共/yy.php php-env:/var/www/html/

```

### ubuntu 
```
docker run --restart=unless-stopped -d     --dns=172.17.0.1     -u=0:0     --shm-size=512m     -p 6901:6901     -e VNC_PW=password     -e VNC_USE_HTTP=0  -e TZ=Asia/Shanghai -v /mnt:/mnt:rslave --name ubuntu "linkease/desktop-ubuntu-standard-arm64:latest"
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
  -v /usr/local/src/appdata/xteve/:/root/.xteve:rw \
  -v /usr/local/src/appdata/xteve/_config/:/config:rw \
  -v /usr/local/src/appdata/xteve/_guide2go/:/guide2go:rw \
  -v /tmp/xteve/:/tmp/xteve:rw \
  -v /usr/local/src/appdata/tvheadend/data/:/TVH \
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
    -e RPC_SECRET=XXXXXX \
    -e RPC_PORT=6800 \
    -e IPV6_MODE=true \
    -p 6800:6800 \
    -e LISTEN_PORT=6888 \
    -p 6888:6888 \
    -p 6888:6888/udp \
    -v /usr/local/src/appdata/aria2/aria2-config:/config \
    -v /mnt/sda1/Public/Downloads:/downloads \
    p3terx/aria2-pro
```
### aria2 webUi控制台
[参考教程](https://p3terx.com/archives/aria2-frontend-ariang-tutorial.html)
```
# bridge 网络模式
docker run -d \
    --name ariang \
    --restart unless-stopped \
    --log-opt max-size=1m \
    -p 6880:6880 \
    p3terx/ariang

# host 网络模式（如果你需要使用 IPv6 网络访问，这是最简单的方式）
docker run -d \
    --name ariang \
    --log-opt max-size=1m \
    --restart unless-stopped \
    --network host \
    p3terx/ariang --port 6880 --ipv6
```

### 安装nextcloud
1. 先安装数据库mariadb：Docker拉取mariadb镜像并创建容器，进入终端，输入下面的命令并回车运行(先别直接复制输入，下方有说明);
```
docker run -d --name mariadb \
    -p 3306:3306 \
    -e MYSQL_ROOT_PASSWORD=752838157w \
    -v /usr/local/src/appdata/mariadb:/var/lib/mysql \
    --restart unless-stopped \	
    mariadb:10.5.12
```
+ 说明：
	+ MYSQL_ROOT_PASSWORD=123456 设置数据库root账户密码，设置为123456，按需修改。

	+ 3344:3306 将mariadb数据库的3306端口映射为3344，按需修改

	+ 连接命令同mysql`mysql -u root -p`


2. 再安装Nextcloud：Docker拉取nextcloud镜像并创建容器，进入终端，输入下面的命令并回车运行(先别直接复制输入，下方有说明);
```
docker run -d --name nextcloud \
  -p 3333:80 \
  -v /mnt/mmc0-4/nextcloud/html:/var/www/html \
  -v /mnt/mmc0-4/nextcloud/data:/var/www/html/data \
  -v /mnt/mmc0-4/nextcloud/apps:/var/www/html/custom_apps \
  -v /mnt/mmc0-4/nextcloud/config:/var/www/html/config \
  --restart unless-stopped \
  nextcloud
```
说明：

/root/nextcloud/html Nextcloud主文件夹的映射目录，按需修改。

/root/nextcloud/data 实际数据的映射目录，按需修改。

/root/nextcloud/apps 安装/修改的应用程序的映射目录，按需修改。

/root/nextcloud/config 本地配置文件的映射目录，按需修改。

3333:80 将nextcloud的访问端口映射为3333，按需修改。

### 安装迅雷
默认端口2345
```
docker run -d --name=xunlei --hostname=mynas --net=host \
-v /usr/local/src/appdata/xunlei:/xunlei/data \
-v /mnt/sda1/media/Movies:/xunlei/nas \
-v /mnt/Public/Downloads:/xunlei/downloads \
--restart=unless-stopped \
--privileged \
registry.cn-shenzhen.aliyuncs.com/cnk3x/xunlei:latest

```
### 安装alist
默认端口5244
```
docker run -d --restart=always \
-v /usr/local/src/appdata/alist:/opt/alist/data \
 -v /mnt/sda1:/opt/alist/mnt \ 
 -p 5244:5244 \
 -e PUID=0 -e PGID=0 \
 -e UMASK=022 \
  --name="alist" \
  xhofe/alist:latest
```
安装后查看密码：
```
docker exec -it alist ./alist admin
```

### mysql
+ 安装
     ```shell
     sudo    docker run --name mysql \
     -v /usr/local/src/mysql:/var/lib/mysql \
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


### photoprism私人相册
```
docker run -d --name photoprism \
 -e PHOTOPRISM_ADMIN_PASSWORD=******* \
 -p 2342:2342 \
 -v /usr/local/src/appdata/photoprism:/photoprism/storage \
 -v /mnt/sda1/media/Photos:/photoprism/originals \
 --restart unless-stopped \
photoprism/photoprism
```

### emby
```
docker run \
--network=bridge \
-p '8097:8096' \
-v /usr/local/src/appdata/emby:/config \
-v /mnt/sda1/media/:/data \
-e TZ="Asia/Shanghai" \
--device /dev/dri:/dev/dri \
-e UID=0 \
-e GID=0 \
-e GIDLIST=0 \
--restart always \
--name emby \
-d lovechen/embyserver:latest
```
+ `1900`与xteve端口冲突
+ `8920`,`7359`与jellyfin冲突


### jackett
```
docker run -d \
  --name=jackett \
  -e PUID=0 \
  -e PGID=0 \
  -e TZ=Asia/Shanghai \
  -e AUTO_UPDATE=true `#optional` \
  -p 9117:9117 \
  -v /usr/local/src/appdata/jacket:/config \
  --restart unless-stopped \
  linuxserver/jackett:latest
```

### qbittorrent
```
docker run -d \
  --name=qbittorrent \
  -e PUID=$UID \
  -e PGID=$GID \
  -e TZ=Asia/Shanghai \
  -e WEBUI_PORT=8081 \
  -p 6881:6881 \
  -p 6881:6881/udp \
  -p 8081:8081 \
  -v /usr/local/src/appdata/qbittorrent/config:/etc/qBittorrent \
  -v /mnt/sda1/Public/Downloads:/downloads \
  --restart unless-stopped \
  linuxserver/qbittorrent:latest
```

### jellyfin
```
docker run -d --name=jellyfin -p 8096:8096 \
	-p 8920:8920 -p 7359:7359/udp \
	-v /usr/local/src/appdata/jellyfin/config:/config \
	-v /mnt/sda1/media:/media \
	-v /usr/local/src/appdata/jellyfin/cache:/cache  \
	-e TZ=Asia/Shanghai -e PUID=0 -e PGID=0 \
	--device=/dev/dri:/dev/dri \
	--restart unless-stopped \
    --add-host=api.themoviedb.org:13.224.161.90 \	
    --add-host=api.themoviedb.org:13.35.8.65 \
    --add-host=api.themoviedb.org:13.35.8.93 \
    --add-host=api.themoviedb.org:13.35.8.6 \
    --add-host=api.themoviedb.org:13.35.8.54 \
    --add-host=image.tmdb.org:138.199.37.230 \
    --add-host=image.tmdb.org:108.138.246.49 \
    --add-host=api.thetvdb.org:13.225.89.239 \
    --add-host=api.thetvdb.org:192.241.234.54 \
jellyfin/jellyfin:latest
```
说明：
+ `--add-host`为容器增加 host 指向，加速海报与影视元数据的搜刮
    也可以在容器控制台输入：
    ```
    echo 13.224.161.90 api.themoviedb.org >> /etc/hosts
    echo 104.16.61.155 image.themoviedb.org >> /etc/hosts
    echo 13.35.67.86 api.themoviedb.org >> /etc/hosts
    echo 54.192.151.79 www.themoviedb.org >> /etc/hosts
    echo 13.225.89.239 api.thetvdb.com >> /etc/hosts
    echo 13.249.175.212 api.thetvdb.com >> /etc/hosts
    echo 13.35.161.120 api.thetvdb.com >> /etc/hosts
    echo 13.226.238.76 api.themoviedb.org >> /etc/hosts
    echo 13.35.7.102 api.themoviedb.org >> /etc/hosts
    echo 13.225.103.26 api.themoviedb.org >> /etc/hosts
    echo 13.226.191.85 api.themoviedb.org >> /etc/hosts
    echo 13.225.103.110 api.themoviedb.org >> /etc/hosts
    echo 52.85.79.89 api.themoviedb.org >> /etc/hosts
    echo 13.225.41.40 api.themoviedb.org >> /etc/hosts
    echo 13.226.251.88 api.themoviedb.org >> /etc/hosts
    echo 185.199.111.133 raw.github.com >> /etc/hosts
    echo 185.199.111.133 raw.githubusercontent.com >> /etc/hosts
    echo 140.82.114.4    github.com >> /etc/hosts
    ```
+ 如果使用 `linuxserver/jellyfin`` 镜像，就把最后一行替换为下行`lscr.io/linuxserver/jellyfin:latest``
+ 如果使用 `nyanmisaka/jellyfin`  镜像，最把最后一行替换为下行`nyanmisaka/jellyfin:latest`` #arm不支持
+ 下载字体改名后，字体替换
    ```
    docker cp DejaVuSans-Bold.ttf  jellyfin:/usr/share/fonts/truetype/dejavu/
    docker cp DejaVuSansMono-Bold.ttf  jellyfin:/usr/share/fonts/truetype/dejavu/
    docker cp DejaVuSansMono.ttf  jellyfin:/usr/share/fonts/truetype/dejavu/
    docker cp DejaVuSans.ttf  jellyfin:/usr/share/fonts/truetype/dejavu/
    docker cp DejaVuSerif.ttf  jellyfin:/usr/share/fonts/truetype/dejavu/
    docker cp DejaVuSerif-Bold.ttf  jellyfin:/usr/share/fonts/truetype/dejavu/
    ```

## 直接安装
### jellyfin
如果尚未安装APT的HTTPS传输，请执行以下操作：
```
sudo apt install apt-transport-https
```

导入GPG签名密钥（由Jellyfin团队签名）：
```
wget -O - https://repo.jellyfin.org/debian/jellyfin_team.gpg.key | sudo apt-key add -
```

在以下位置添加存储库配置`/etc/apt/sources.list.d/jellyfin.list`：

```
echo "deb [arch=$( dpkg --print-architecture )] https://repo.jellyfin.org/debian $( lsb_release -c -s ) main" | sudo tee /etc/apt/sources.list.d/jellyfin.list
```

也可以直接编辑 /etc/apt/sources.list.d/jellyfin.list
```
deb [arch=armhf] https://repo.jellyfin.org/debian buster main
```


注意：

debian 支持的版本是stretch和buster。具体要根据你树莓派上的版本来设置（通过命令 lsb_release -c -s 查看）。

更新APT存储库：
```
sudo apt update
```

安装Jellyfin
```
sudo apt install jellyfin
```

使用您选择的工具管理Jellyfin系统服务：
```
sudo service jellyfin status
sudo /etc/init.d/jellyfin stop
sudo systemctl restart jellyfin
```

### 安装Alist
```
curl -fsSL "https://alist.nn.ci/v3.sh" | bash -s install
```

## ipv6 cloudflare ddns
[参考链接](https://zhuanlan.zhihu.com/p/69379645)
```
curl -s -X GET "https://api.cloudflare.com/client/v4/zones/<刚才获取的 Zone ID>/dns_records?type=AAAA&name=<DDNS 域名>&content=127.0.0.1&page=1&per_page=100&order=type&direction=desc&match=any" \
    -H "X-Auth-Email: <Cloudflare 账号的邮箱地址>" \
    -H "X-Auth-Key: <刚才获取的 API Key>" \
    -H "Content-Type: application/json" \
    | python -m json.tool
```

```
#!/bin/bash

IP6=$(ip -6 addr show dev eth0 | grep global | awk '{print $2}' | awk -F "/" '{print $1}' | awk 'length < shortest || shortest == 0 {shortest = length; shortest_line = $0} END {print shortest_line}')
echo $IP6
API_KEY=""
ZONE_ID=""
RECORD_ID=""
RECORD_NAME=""
EMAIL=""
curl -X PUT "https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/$RECORD_ID" \
     -H "X-Auth-Key: $API_KEY" \
     -H "X-Auth-Email: $EMAIL" \
     -H "Content-Type: application/json" \
     --data "{\"type\":\"AAAA\",\"name\":\"$RECORD_NAME\",\"content\":\"$IP6\",\"ttl\":120,\"proxied\":false}"
```
定时执行
```
crontab -e
* * * * * /etc/ipv6-ddns.sh > /dev/null 2>&1
```





