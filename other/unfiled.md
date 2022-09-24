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

## openwrt关闭led灯

保存为 `/etc/rc.d/S99turnoffled`

```shell
#!/bin/ash
for i in `ls /sys/class/leds`
do cd /sys/class/leds
cd $i
echo 0 > brightness
done
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


## windows问题记录

### windows保护历史记录删除

删除windows保护历史记录，路径：`C:\ProgramData\Microsoft\Windows Defender\Scans\History\Service\DetectionHistory`，删除该目录下所有文件

### 上帝模式

新建文件夹，重命名输入 `上帝模式.{ED7BA470-8E54-465E-825C-99712043E01C}`

### 防止浏览器主页篡改

任务栏浏览器主页被篡改，右键属性就会看到目标后，有一串网址，删除解决，路径：`C:\Users\用户名\AppData\Roaming\Microsoft\Internet Explorer\Quick Launch\User Pinned\TaskBar`

### 高分辨率远程桌面低分辨率屏幕内容过小解决办法

1. 修改注册表：用运行-regedit编辑注册表，找到：`HKEY_LOCAL_MACHINE > SOFTWARE > Microsoft > Windows > CurrentVersion > SideBySide`新建`DWORD`，命名`PreferExternalManifest`，并双击设置值为`1`.
2. 新建文件`mstsc.exe.manifest`，这个文件放到指定路径中：`C:\Windows\System32`

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" xmlns:asmv3="urn:schemas-microsoft-com:asm.v3" manifestVersion="1.0">
    <dependency>
        <dependentAssembly>
        <assemblyIdentity type="win32" name="Microsoft.Windows.Common-Controls" version="6.0.0.0" processorArchitecture="*" publicKeyToken="6595b64144ccf1df" language="*">

    </assemblyIdentity>
        </dependentAssembly>
    </dependency>
    <dependency>
        <dependentAssembly>
        <assemblyIdentity type="win32" name="Microsoft.VC90.CRT" version="9.0.21022.8" processorArchitecture="amd64" publicKeyToken="1fc8b3b9a1e18e3b">

    </assemblyIdentity>
        </dependentAssembly>
    </dependency>
    <trustInfo xmlns="urn:schemas-microsoft-com:asm.v3">
        <security>
        <requestedPrivileges>
            <requestedExecutionLevel level="asInvoker" uiAccess="false"/>
        </requestedPrivileges>
        </security>
    </trustInfo>
    <asmv3:application>
        <asmv3:windowsSettings xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">
        <ms_windowsSettings:dpiAware xmlns:ms_windowsSettings="http://schemas.microsoft.com/SMI/2005/WindowsSettings">false</ms_windowsSettings:dpiAware>
        </asmv3:windowsSettings>
    </asmv3:application>
    </assembly>
    ```

### 双系统导致时间不同步问题
`Win+R`打开运行窗口，输入`regedit` 回车，找到`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control \TimeZoneInformation`，右键新建`New` > `DWORD (32-bit) Value`，命名为`RealTimeIsUniversal`，键值为`1`

## 解决使用代理时无法访问 Microsoft Store 0x80131500

```shell
foreach ($n in (get-appxpackage).packagefamilyname) {checknetisolation loopbackexempt -a -n="$n"}
```

### hosts文件路径

`C:\Windows\System32\drivers\etc`

### windows下隐藏文件
```shell
attrib +s +a +h +r E:\hide
attrib -s -a -h -r E:\hide
```