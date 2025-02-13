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

## app开源
1. 影视app [release](https://github.com/FongMi/Release/tree/okjack/apk/release)
      + [蜂蜜影视tv版](https://github.com/FongMi/Release/blob/okjack/apk/release/leanback-python-armeabi_v7a.apk)
      + [手机版](https://github.com/FongMi/Release/blob/okjack/apk/release/mobile-python-arm64_v8a.apk)
2. musicFree [release](https://github.com/maotoumao/MusicFree/releases/)
3. 阅读 [release](https://github.com/gedoor/legado/releases)


