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

## windows

1. 删除windows保护历史记录，路径：`C:\ProgramData\Microsoft\Windows Defender\Scans\History\Service\DetectionHistory`，删除该目录下所有文件
2. 新建文件夹，重命名输入 `上帝模式.{ED7BA470-8E54-465E-825C-99712043E01C}`
3. 任务栏浏览器主页被篡改，右键属性就会看到目标后，有一串网址，删除解决，路径：`C:\Users\用户名\AppData\Roaming\Microsoft\Internet Explorer\Quick Launch\User Pinned\TaskBar`

## batocera

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

## 解决使用代理时无法访问 Microsoft Store 0x80131500

```shell
foreach ($n in (get-appxpackage).packagefamilyname) {checknetisolation loopbackexempt -a -n="$n"}
```
