# 树莓派使用记录
## 下载
选择自己要安装的镜像下载

## 校验包，解压
```
sha1sum 2013-09-25-wheezy-raspbian.zip
unzip  2013-09-25-wheezy-raspbian.zip
```
查看当前哪些设备已经挂载,` df -h`，插入u盘或sd卡再执行一次
为了防止在写入镜像的时候有其他读取或写入，我们需要卸载设备。两个分区都要卸载。

```   
umount  /dev/sdb1
umount  /dev/sdb2
```
## xz烧录命令(该命令尝试失败)
```
sudo xz -cd kali-2017.3-rpi3-nexmon.img.xz> /dev/sdb
```
查看烧录进度`sudo pkill -USR1 -n -x xz`

## img格式镜像烧录命令如下（亲测成功）
```
sudo dd bs=4M if=2013-09-25-wheezy-raspbian.img of=/dev/sdb
```
查看烧录进度`sudo pkill -USR1 -n -x dd`
## 卸载软件
1. 卸载但不删除配置
```
apt-get remove packagename
```
2. 卸载并删除配置
```
apt-get purge packagename
```

## 基础命令

[原链接](https://shumeipai.nxez.com/2015/01/03/raspberry-pi-software-installation-and-uninstallation-command.html)

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

```
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
```

> dpkg安裝的可以用apt卸載，反之亦可。

## aptitude

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
