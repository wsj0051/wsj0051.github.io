# dpkg命令
dpkg命令的英文全称是“Debian package”，dpkg是Debian的一个底层包管理工具，主要用于对已下载到本地和已安装的软件包进行管理。

dpkg这个机制最早由Debian Linux社区所开发出来的，通过dpkg的机制，Debian提供的软件就能够简单的安装起来，同时能提供安装后的软件信息，实在非常不错。只要派生于Debian的其它Linux distributions大多使用dpkg这个机制来管理，包括B2D，Ubuntu等。
## dpkg软件包相关文件介绍

`/etc/dpkg/dpkg.cfg` dpkg包管理软件的配置文件【Configuration file with default options】

`/var/log/dpkg.log`  dpkg包管理软件的日志文件【Default log file (see /etc/dpkg/dpkg.cfg(5) and option --log)】

`/var/lib/dpkg/available`  存放系统所有安装过的软件包信息【List of available packages.】

`/var/lib/dpkg/status`   存放系统现在所有安装软件的状态信息

`/var/lib/dpkg/info`   记安装软件包控制目录的控制信息文件


| 参数| 说明|
| :-------- | :--------| 
|-i	 |安装软件包 |
|-r |	删除软件包 |
|-l |	显示已安装软件包列表 |
|-L |	显示于软件包关联的文件 |
|-c |	显示软件包内文件列表 |
## 参考实例

安装包：
```
dpkg -i package.deb 
```
删除包：
```
dpkg -r package.deb  
```
`dpkg -r package-name`  # --remove， 移除软件包，但保留其配置文件  
`dpkg -P package-name ` # --purge， 清除软件包的所有文件（removes everything, including conffiles）

列出当前已安装的包：
```
dpkg -l 
```
每条记录对应一个软件包，注意每条记录的第一、二、三个字符，这就是软件包的状态标识，后边依此是软件包名称、版本号和简单描述。

1. 第一字符为期望值(Desired=Unknown/Install/Remove/Purge/Hold)，它包括：  
  u  Unknown状态未知,这意味着软件包未安装,并且用户也未发出安装请求.  
  i  Install用户请求安装软件包.  
  r  Remove用户请求卸载软件包.  
  p  Purge用户请求清除软件包.  
  h  Hold用户请求保持软件包版本锁定.  

2. 第二列,是软件包的当前状态(Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend)  
  n  Not软件包未安装.  
  i  Inst软件包安装并完成配置.  
  c  Conf-files软件包以前安装过,现在删除了,但是它的配置文件还留在系统中.  
  u  Unpacked软件包被解包,但还未配置.  
  f  halF-conf试图配置软件包,但是失败了.  
  h  Half-inst软件包安装,但是但是没有成功.  
  w trig-aWait触发器等待  
  t Trig-pend触发器未决  

3. 第三列标识错误状态,第一种状态标识没有问题,为空. 其它符号则标识相应问题（Err?=(none)/Reinst-required (Status,Err: uppercase=bad)）  
  h  软件包被强制保持,因为有其它软件包依赖需求,无法升级.  
  r  Reinst-required，软件包被破坏,可能需要重新安装才能正常使用(包括删除).  
  x  软包件被破坏,并且被强制保持.  

4. 案例说明：
  ii —— 表示系统正常安装了该软件  
  pn —— 表示安装了该软件，后来又清除了  
  un —— 表示从未安装过该软件  
  iu —— 表示安装了该软件，但是未配置  
  rc —— 该软件已被删除，但配置文件仍在  
5. dpkg子命令
为了方便用户使用，dpkg不仅提供了大量的参数选项, 同时也提供了许多子命令。比如：  
`dpkg-deb、dpkg-divert、dpkg-query、dpkg-split、dpkg-statoverride、start-stop-daemon`

列出deb包的内容：
```
[root@linuxcool ~]# dpkg -c package.deb 
```
配置：
```
[root@linuxcool ~]# dpkg --configure package 
```
查询
```
dpkg -l package-name-pattern  # --list, 查看系统中软件包名符合pattern模式的软件包
dpkg -L package-name  # --listfiles, 查看package-name对应的软件包安装的文件及目录
dpkg -p package-name  # --print-avail, 显示包的具体信息
dpkg -s package-name  # --status, 查看package-name（已安装）对应的软件包信息
dpkg -S filename-search-pattern  # --search, 从已经安装的软件包中查找包含filename的软件包名称
```
更多dpkg的使用方法可在命令行里使用`man dpkg`来查阅 或直接使用`dpkg --help`。
