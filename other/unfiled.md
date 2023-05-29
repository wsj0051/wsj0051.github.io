# 未分类

## Jenkins

- 插件下载地址
  - [hpi插件地址](http://updates.jenkins-ci.org/latest/)
  - [Theme插件](http://wiki.jenkins-ci.org/display/JENKINS/Simple+Theme+Plugin)
- 页面自定义
  - 修改jar包`jenkins/WEB-INF/lib/jenkins-core-1.651.3.jar`文件中的`lib/layout/layout.jelly`

## maven安装本地jar

```shell
mvn install:install-file -Dfile=junit-4.8.1.jar -DgroupId=junit -DartifactId=junit -Dversion=4.8.1 -Dpackaging=jar
```

## vscode 正则匹配空白行

```shell
^\s*(?=\r?$)\n
```

## BATOCERA

配置文件

```text
wifi.enabled=1
wifi.ssid=wifi1-name
wifi.key=********
wifi2.ssid=wifi2-name
wifi2.key=********
wifi3.ssid=wifi1-name
wifi3.key=********
```

## 康佳电视 - LED55K36U 刷机

1. 电视型号：
    - 主程序软件：99015826
    - 屏幕参数：99016671

2. 刷机步骤
    - 主程序软件，包名：`M638Upgrade.bin`
      - 将对应的刷机文件放到u盘根目录（u盘要fat32格式，尽量不要用内存卡，刷机文件不要改名）
      - 断电，按住音量+加键(有些机型是信源键)，开电

3. 若刷机后出现双屏
    - 刷屏幕参数，包名： `M638PanelUpgrade.bin`
      - 按遥控器的音量减键不放，然后通电。


## windows记录

### windows cmd查看并设置代理

1. 设置socks代理
    ```
    set ALL_PROXY socks5://127.0.0.1:10808
    ```

2. 配置http代理
    ```
    set http_proxy=http://127.0.0.1:10809
    set https_proxy=http://127.0.0.1:10809
    ```

3. 查看代理
    ```
      netsh winhttp show proxy
    ```

4. 清除代理
    ```
      netsh winhttp reset proxy
    ```


### 上帝模式

新建文件夹，重命名为 `上帝模式.{ED7BA470-8E54-465E-825C-99712043E01C}`

### 浏览器主页被篡改修复

任务栏浏览器主页被篡改，右键属性就会看到目标后，有一串网址，删除解决，路径：`C:\Users\用户名\AppData\Roaming\Microsoft\Internet Explorer\Quick Launch\User Pinned\TaskBar`

### 高分辨率远程桌面低分辨率屏幕内容过小解决办法

1. 修改注册表：用运行-regedit编辑注册表，找到：`HKEY_LOCAL_MACHINE > SOFTWARE > Microsoft > Windows > CurrentVersion > SideBySide`新建`DWORD`，命名`PreferExternalManifest`，并双击设置值为`1`.
2. 创建文件[mstsc.exe.manifest](./mstsc.exe.manifest)，并放到该路径下：`C:\Windows\System32`


### windows下隐藏文件
```shell
attrib +s +a +h +r E:\hide
attrib -s -a -h -r E:\hide
```
### 路径
1. win11当前壁纸路径`C:\Users\用户名\AppData\Roaming\Microsoft\Windows\Themes\CachedFiles\`

2. hosts文件路径`C:\Windows\System32\drivers\etc`

3. windows保护历史记录删除`C:\ProgramData\Microsoft\Windows Defender\Scans\History\Service\DetectionHistory`
   
## Openwrt路由器

### 查看openwrt已连接网络ip

使用DHCP客户端查看mac地址、ip信息
```
cat /tmp/dhcp.leases
```
通过arp查看ip、mac地址、端口
```
cat /proc/net/arp
```

### openwrt关闭led灯

保存为 `/etc/rc.d/S99turnoffled`

```shell
#!/bin/ash
for i in `ls /sys/class/leds`
do cd /sys/class/leds
cd $i
echo 0 > brightness
done
```


## git设置代理

### 设置全局代理
```
//http
git config --global https.proxy http://127.0.0.1:1080
//https
git config --global https.proxy https://127.0.0.1:1080
//使用socks5代理的 例如ss，ssr 1080是windows下ss的默认代理端口,mac下不同，或者有自定义的，根据自己的改
git config --global http.proxy socks5://127.0.0.1:1080
git config --global https.proxy socks5://127.0.0.1:1080
//取消全局代理
git config --global --unset http.proxy
git config --global --unset https.proxy
```

### 只对github.com使用代理，其他仓库不走代理
```
git config --global http.https://github.com.proxy socks5://127.0.0.1:10808
git config --global https.https://github.com.proxy socks5://127.0.0.1:10808
//取消github代理
git config --global --unset http.https://github.com.proxy
git config --global --unset https.https://github.com.proxy
```

### ssh协议代理
```
//对于使用git@协议的，可以配置socks5代理
//在~/.ssh/config 文件后面添加几行，没有可以新建一个
//socks5
Host github.com
User git
ProxyCommand connect -S 127.0.0.1:10808 %h %p

//http || https
Host github.com
User git
ProxyCommand connect -H 127.0.0.1:1080 %h %p
```