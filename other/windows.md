# Windows使用记录

## cmd查看并设置代理

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


## 上帝模式

新建文件夹，重命名为 `上帝模式.{ED7BA470-8E54-465E-825C-99712043E01C}`

## 浏览器主页被篡改修复

任务栏浏览器主页被篡改，右键属性就会看到目标后，有一串网址，删除解决，路径：`C:\Users\用户名\AppData\Roaming\Microsoft\Internet Explorer\Quick Launch\User Pinned\TaskBar`

## 高分辨率远程桌面低分辨率屏幕内容过小解决办法

1. 修改注册表：用`cmd`运行`regedit`编辑注册表，找到：`HKEY_LOCAL_MACHINE > SOFTWARE > Microsoft > Windows > CurrentVersion > SideBySide`新建`DWORD`，命名`PreferExternalManifest`，并双击设置值为`1`.
2. 创建文件[mstsc.exe.manifest](../assets/mstsc.exe.manifest)，并放到该路径下：`C:\Windows\System32`


## 隐藏文件
```shell
attrib +s +a +h +r E:\hide
attrib -s -a -h -r E:\hide
```

## 通用路径

1. win11当前壁纸路径`C:\Users\用户名\AppData\Roaming\Microsoft\Windows\Themes\CachedFiles\`
2. hosts文件路径`C:\Windows\System32\drivers\etc`
3. windows保护历史记录删除`C:\ProgramData\Microsoft\Windows Defender\Scans\History\Service\DetectionHistory`
4. 开机启动`C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup`

## wsl
1. `powershell`管理员身份打开，执行以下命令
    ```
    wsl --install
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
    wsl --set-default-version 2
    ```
2. [安装适用于 x64 计算机的 WSL2 Linux 内核更新包](  https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
)

3. 在安装的目录下执行以下命令：
    ```
    Invoke-WebRequest -Uri https://wsldownload.azureedge.net/Ubuntu_2004.2020.424.0_x64.appx -OutFile Ubuntu20.04.appx -UseBasicParsing
    Rename-Item .\Ubuntu20.04.appx Ubuntu.zip
    Expand-Archive .\Ubuntu.zip -Verbose
    cd .\Ubuntu\
    .\ubuntu2004.exe
    ```

### 查看当前安装的wsl
```
wsl --list
```
### 注销卸载当前安装的子系统
```
wsl --unregister Ubuntu
```
### 设置代理
```
adb connect 127.0.0.1:58526 && adb shell "settings put global http_proxy 'ip route list match 0 table all scope global | cut -F3'：10808"
```
