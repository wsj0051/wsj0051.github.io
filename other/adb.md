# adb

## adb 连接安卓

```shell
adb connect 127.0.0.1:58526
```

## 安装apk

```shell
adb install 目录/安装包.apk
```
## 推送文件

```shell
adb push ./Download/2023-02-28-231021 /storage/emulated/0/tvbox_backup
```
## 使用adb fastboot 线刷

1. 安装adb fastboot工具

   ```shell
   sudo apt install adb fastboot
   ```

2. 通过USB将您的设备连接到电脑，并验证手机连接

   ```shell
   adb devices
   ```

3. 重启到fastboot模式

   ```shell
   adb reboot-bootloader
   ```

4. 以小米为例，下载官方线刷包后解压[链接](http://www.miui.com/shuaji-393.html)

   ```shell
   tar zxvf clover_images_V10.3.2.0.ODJCNXM_20190515.0000.00_8.1_cn_4388d99e5e.tgz
   ```

5. 进入解压后的目录

   ```shell
   cd clover_images_V10.3.2.0.ODJCNXM_20190515.0000.00_8.1_cn
   ```

6. 开始刷机
   * 修改为可执行`chmod +x flash_all.sh`
   * fastboot模式下再次验证手机连接`fastboot devices`
   * 执行刷机脚本[^1]`sudo sh flash_all.sh`

## 通过 fastboot 安装 Recovery

1. 下载 Recovery - 例如 [TWRP](https://twrp.me/)。

   * 您可以前往 [魔趣下载站](https://download.mokeedev.com) 的设备页面，点击 **Recovery 下载**。
   * 如果没有找到适用于您设备的 Recovery，请借助互联网搜索设备维护者或资深玩家发布的教程。

2. 通过USB将您的设备连接到电脑。

3. 在电脑上打开命令提示符（Windows）或 终端 （Linux 或 macOS）并输入：

   ```shell
   adb reboot bootloader
   ```

   您也可以通过组合键启动 fastboot 模式：

   * 关闭设备后，按住音量调低 + 电源键，直到屏幕上方出现“FASTBOOT”字样，然后松开。

4. 一旦设备处于 fastboot 模式，请通过键入以下内容验证您的 PC 是否找到它：

   ```shell
   fastboot devices
   ```

5. 将 Recovery 刷入到您的设备。

   ```shell
   fastboot flash recovery twrp-x.x.x-x-x.img # 仅刷入
   fastboot boot .\twrp-x.x.x-x-x.img  # 刷入重启到recovery
   ```

6. 现在进入 Recovery 模式以验证安装：

   * 关闭设备后，按住音量调高 + 电源键，直到进入 Recovery 模式，然后松开。

## 通过 Recovery 安装魔趣ROM

1. 下载你想要安装的魔趣ROM包。
   * 可选项，下载第三方扩展包，例如 [OpenGapps](https://opengapps.org/)

2. 如果您尚未进入 Recovery 模式，请重启到 Recovery 模式。
   * 关闭设备后，按住音量调高 + 电源键，直到进入 Recovery 模式，然后松开。

[^1]: 该脚本删除所有数据，可以选择其他sh结尾的脚本，根据名字可以猜到大概意思

## fastboot命令
1. 列出fastboot设备
   ```
   fastboot devices
   ```
2. 重启相关
   ```shell
   fastboot reboot            #重启⼿机
   fastboot reboot-bootloader    #重启到bootloader模式,其实就是再次进入fastboot
   ```
3. 擦除相关（erase）
   ```shell
   fastboot erase boot    #擦除boot分区(擦了引导就没了,会卡在第一屏,)
   fastboot erase recovery   #擦除recovery分区
   fastboot erase system    #擦除system分区(擦了系统就没了,会卡在第二屏)
   fastboot erase userdata    #擦除userdata分区(可擦,清空数据用)
   fastboot erase cache    #擦除cache分区(可擦,清空数据用)
   ```
4. 写⼊分区（flash）
   ```shell
   fastboot flash boot boot.img    #写⼊boot分区
   fastboot flash init_boot ****.img #写入initboot 安卓13用来刷magisk修补后的文件获得root权限
   fastboot flash recovery recovery.img    写⼊recovery分
   fastboot flash system system.img    #写⼊system分区
   ```

## win11 wsa安卓相关
```shell
adb connect 127.0.0.1:58526
adb shell settings get global http_proxy
adb shell "settings put global http_proxy `ip route list match 0 table all scope global | cut -F3`:10809"
adb shell settings put global http_proxy :0
```