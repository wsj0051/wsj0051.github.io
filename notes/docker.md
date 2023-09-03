# wsl2 ubuntu docker

## 安装

### 修改默认源

```shell
cp /etc/apt/sources.list /etc/apt/sourses.list.bak
```

编辑文件

```shell
vim /etc/apt/sources.list
```

删除原有内容并替换为：

```text
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

更新源

```shell
apt-get update
apt-get upgrade
```

### 安装软件包以允许 apt 通过 HTTPS 使用存储库

```shell
apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

### 添加 Docker 的官方 GPG 密钥

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

### 设置稳定的存储库

```shell
add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

### 更新 apt 包索引

```shell
apt-get update
docker --version
sudo gpasswd -a 用户名 docker 
```

## docker镜像源修改

`vim /etc/docker/daemon.json`添加以下内容

```json
{
"registry-mirrors":["https://hub-mirror.c.163.com","https://registry.aliyuncs.com","https://registry.docker-cn.com","https://docker.mirrors.ustc.edu.cn"]
}
```

## 启动、停止与重启

```shell
sudo service docker start
sudo service docker stop
sudo service docker restart
```

## 镜像相关

### 查看镜像

```shell
docker images
```

### 搜索镜像

```shell
docker search 镜像名
```

### 下载

```shell
docker pull 镜像名
```

### 删除镜像

按镜像id删除

```shell
docker rmi 镜像id
```

删除所有镜像

```shell
docker rmi `docker images -q`
```

## 容器相关

### 查看容器

查看正在运行的容器

```shell
docker ps
```

查看所有容器

```shell
docker ps -a
```

查看停止的容器

```shell
docker ps -f status=exited 
```

### 创建与启动容器

创建容器命令：`docker run`
-i: 表示运行容器
-t: 表示启动后进入其命令行
--name: 为容器命名
-v: 表示目录的映射关系（宿主机目录:容器的目录）
-d: 创建一个守护式容器在后台运行
-p: 表示端口映射（宿主机端口：容器内的映射端口）

1. 交互式方式创建容器

   ```shell
   docker run -it --name=容器名称 镜像名称:标签 /bin/bash
   ```

2. 守护式创建容器

   ```shell
   docker run -di --name=容器名称 镜像名称:标签
   ```

3. 登录守护式容器方式

    ```shell
    docker exec -it 容器名称或容器id /bin/bash
    ```

4. 启动和停止容器

   ```shell
   docker start 容器名称或容器id
   docker stop 容器名称或容器id
   ```

5. 将文件拷贝到容器

   ```shell
   docker cp 要拷贝的文件或目录 容器名称:容器目录
   ```  

6. 将文件从容器拷贝出来

    ```shell
    docker cp 要拷贝的容器名称:要拷贝的容器目录 本地目录 
    ```  

7. 目录挂载

   ```shell
   docker run -di -v 宿主机目录:容器目录 --name=自定义容器名 镜像名:tag
   ```

### 查看容器的ip地址

{% raw %}

```shell
docker inspect 容器名称（容器id）
docker inspect --format='{{.NetworkSettings.IPAddress}}' 容器名
```

{% endraw %}

### 删除容器

```shell
docker stop 容器名
docker rm 容器名
```

## 备份与迁移

1. 保存为镜像

    ```shell
    docker commit nginx mynginx
    ```

2. 镜像备份，保存为tar

    ```shell
    docker save -o mynginx.tar mynginx
    ```

3. 恢复与迁移，加载tar文件

    ```shell
    docker load -i mynginx.tar
    ```

## Dockerfile

Dockerfile是一系列命令和参数构成的脚本，这些命令用于基础镜像并最终创建一个新的镜像。

| 命令 | 作用 |
|:--|---------|
|FROM image_name:tag|定义了使用哪个基础镜像启动构建流程|
|MAINTAINER user_name|声明镜像的创建者|
|ENV key value|设置环境变量|
|RUN command|Dockerfile的核心部分|
|ADD source_dir/file dest_dir/file|将宿主机文件复制到容器内，压缩文件自动解压|
|WORKDIR path_dir|设置工作目录|

以jdk8为例创建Dockerfile

```shell
mkdir -p /user/local/docker-jdk8
vi Dockerfile
```

以下为Dockerfile内容：

```text
FROM ubuntu
MAINTAINER wsj0051
WORKDIR /usr
RUN mkdir /usr/local/java
ADD jdk*.tar.gz /usr/local/java/
ENV JAVA_HOME /usr/local/java/jdk1.8
ENV JRE_HOME $JAVA_HOME/jre
ENV CLASSPATH $JAVA_HOME/bin/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH
ENV PATH $JAVA_HOME/bin:$PATH
```

执行命令构建镜像

```shell
docker build -t='jdk1.8' .
```

## Docker私有仓库

1. 拉取私有仓库镜像

   ```shell
   docker pull registry
   ```

2. 启动私有仓库

   ```shell
   docker run -di --name=registry -p 5000:5000 registry
   ```

3. 浏览器输入地址<http://ip:5000/v2/_catalog>看到{"repositories":[]}表示私有仓库搭建成功并且内容为空

4. 修改`/etc/docker/daemon.json`

   ```shell
   {"insecure-registries":["ip:5000"]}
   ```

5. 将镜像上传到私有仓库

   ```shell
   docker tag jdk1.8 ip:5000/jdk1.8
   docker push ip:5000/jdk1.8
   ```

## 常用应用安装

### tomcat

```shell
docker run -di --name=mytomcat -p 8080:8080 -v /usr/local/webapps:/usr/local/tomcat/webapps tomcat:7-jre7
```

### nginx

```shell
docker pull nginx
docker run -di --name=nginx -p 80:80 nginx
docker exec -it nginx /bin/bash
docker cp html nginx:/usr/share/nginx/ --将本地的html文件夹复制到nginx下
```

### redis

```shell
docker run -di --name=myredis -p 5379:6379 redis
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
  -e "DOCKER_USER=$USER" \
  -e PASSWORD=******** \
codercom/code-server:latest
```
### php-env肥羊
```
docker run -d --restart unless-stopped \
--privileged=true -p 5678:80 \
--name php-env \
-v /usr/local/src/appdata/php-env:/var/www/html \
youshandefeiyang/php-env:arm64
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
#### 使用mariadb
1. 先安装数据库mariadb：Docker拉取mariadb镜像并创建容器，进入终端，输入下面的命令并回车运行(先别直接复制输入，下方有说明);
```
docker run -d --name mariadb \
    -p 3306:3306 \
    -e MYSQL_ROOT_PASSWORD=****** \
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

#### 使用mysql

```shell
# mysql
docker run -d --name mysql \
    -v /usr/local/src/appdata/mysql:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=****** \
    -e MYSQL_DATABASE=nextcloud \
    -e MYSQL_USER=nextcloud \
    -e MYSQL_PASSWORD=****** \
    -p 3307:3306 \
    --restart=always \
   -d mysql/mysql-server
```
+ `-e` 代表添加环境变量 `MYSQL_ROOT_PASSWORD` 是root用户的登录密码
+ 进入mysql容器，并登陆mysql
     ```
     docker exec -it mysql bash
     mysql -u root -p
     ```
+ 创建用户，赋予权限
     ```
     Create user 'nextcloud'@'%' identified by '*******';
     grant all privileges on nextcloud.* to 'nextcloud'@'%';
     ```
+ 开启远程访问权限
     ```
     use mysql;
     select host,user from user;
     alter user 'root'@'%' identified with mysql_native_password by '******'; 
     flush privileges;
     ```
 + 开机启动
     ```
     sudo docker update mysql --restart=always

     ```


树莓派64位系统安装nextcloud

```
docker run -d --name nextcloud \
    -v /usr/local/src/appdata/nextcloud/data:/var/www/html \
    --link mysql:mysql \
    --restart=always \
    --privileged=true \
    -p 8888:80 arm64v8/nextcloud    
    
```
+ 修改配置文件
    ```shell
    mkdir /data/nextcloud/config
    sudo docker cp nextcloud:/var/www/html/config/config.sample.php  /data/nextcloud/config/config.sample.php
    sudo docker cp /data/nextcloud/config/config.php  nextcloud:/var/www/html/config/config.php
    nano /data/nextcloud/config/config.sample.php
    ```
+ 修改`trusted_domains`里的值
    ```txt
    1 => preg_match('/cli/i',php_sapi_name())?'127.0.0.1':$_SERVER['SERVER_NAME']
    ```



### 安装迅雷
默认端口2345
```
docker run -d --name=xunlei --hostname=mynas --net=host \
-v /usr/local/src/appdata/xunlei:/xunlei/data \
-v /mnt/sda1/media/Movies:/xunlei/nas \
-v /mnt/sda1/Public/Downloads:/xunlei/downloads \
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

## 设置代理
### Docker 代理
在执行docker pull时，是由守护进程dockerd来执行。因此，代理需要配在dockerd的环境中。而这个环境，则是受systemd所管控，因此实际是systemd的配置。
```
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo touch /etc/systemd/system/docker.service.d/proxy.conf
```
在这个proxy.conf文件（可以是任意*.conf的形式）中，添加以下内容：
```
[Service]
Environment="HTTP_PROXY=http://proxy.example.com:8080/"
Environment="HTTPS_PROXY=http://proxy.example.com:8080/"
Environment="NO_PROXY=localhost,127.0.0.1,.example.com"

```
其中，proxy.example.com:8080 要换成可用的免密代理。通常使用 cntlm 在本机自建免密代理，去对接公司的代理。可参考《Linux下安装配置Cntlm 代理》。

### Container 代理
在容器运行阶段，如果需要代理上网，则需要配置 ~/.docker/config.json。以下配置，只在Docker 17.07及以上版本生效。
```
{
 "proxies":
 {
   "default":
   {
     "httpProxy": "http://proxy.example.com:8080",
     "httpsProxy": "http://proxy.example.com:8080",
     "noProxy": "localhost,127.0.0.1,.example.com"
   }
 }
}
```

这个是用户级的配置，除了 proxies，docker login 等相关信息也会在其中。而且还可以配置信息展示的格式、插件参数等。

此外，容器的网络代理，也可以直接在其运行时通过 -e 注入 http_proxy 等环境变量。这两种方法分别适合不同场景。config.json 非常方便，默认在所有配置修改后启动的容器生效，适合个人开发环境。在CI/CD的自动构建环境、或者实际上线运行的环境中，这种方法就不太合适，用 -e 注入这种显式配置会更好，减轻对构建、部署环境的依赖。当然，在这些环境中，最好用良好的设计避免配置代理上网。

### Docker Build 代理
虽然 docker build 的本质，也是启动一个容器，但是环境会略有不同，用户级配置无效。在构建时，需要注入 http_proxy 等参数。
```
docker build . \
    --build-arg "HTTP_PROXY=http://proxy.example.com:8080/" \
    --build-arg "HTTPS_PROXY=http://proxy.example.com:8080/" \
    --build-arg "NO_PROXY=localhost,127.0.0.1,.example.com" \
    -t your/image:tag
```

注意：无论是 docker run 还是 docker build，默认是网络隔绝的。如果代理使用的是 localhost:3128 这类，则会无效。这类仅限本地的代理，必须加上 --network host 才能正常使用。而一般则需要配置代理的外部IP，而且代理本身要开启 Gateway 模式。

### 重启生效
代理配置完成后，reboot 重启当然可以生效，但不重启也行。

docker build 代理是在执行前设置的，所以修改后，下次执行立即生效。Container 代理的修改也是立即生效的，但是只针对以后启动的 Container，对已经启动的 Container 无效。

dockerd 代理的修改比较特殊，它实际上是改 systemd 的配置，因此需要重载 systemd 并重启 dockerd 才能生效。
```
sudo systemctl daemon-reload
sudo systemctl restart docker
```