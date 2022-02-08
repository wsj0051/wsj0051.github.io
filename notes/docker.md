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

### mysql

```shell
docker pull mysql:5.7.33
docker run -di --name=mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7.33
```

`-e` 代表添加环境变量 `MYSQL_ROOT_PASSWORD` 是rrot用户的登录密码

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
