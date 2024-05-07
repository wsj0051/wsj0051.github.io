# Openwrt相关
## 查看openwrt设备的cpu架构
```
cat /etc/os-release |grep ARCH
```
## 查看openwrt已连接网络ip

使用DHCP客户端查看mac地址、ip信息
```
cat /tmp/dhcp.leases
```
通过arp查看ip、mac地址、端口
```
cat /proc/net/arp
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

## 光猫桥接ipv6开启
设置光猫为桥接模式，打开`IPV4/IPV6`

### 移动光猫超级管理员
移动光猫账号∶CMCCAdmin 密码∶ aDm8H%MdA 

### 设置软路由或者路由器
1. 有软路由情况下：
   + 网络——接口——IPV6 ULA清空
   + 网络——接口——WAN口——使用内置IPV6管理取消勾选——Obtain IPv6-Address项目确保是自动状态
   + 网络——接口——lan口——DHCP——IPV6设置——高级设置——取消勾选使用内置IPV6管理——取消勾选强制链路
   + 通告的DNS服务器（添加谷歌IPV6DNS）：
   + 2001:4860:4860::8888
   + 2001:4860:4860::8844
   + 在DHCP/DNS——高级设置——取消勾选禁止解析IPV6 DNS记录
   + 设置完成后一定要重启软路由

2. 测试IPV6：
   + https://ipv6-test.com/
   + https://test-ipv6.com/index.html.zh_CN

### 非桥接ipv6开启
```
config dhcp 'lan'
        option interface 'lan'
        option start '100'
        option limit '150'
        option leasetime '12h'
        option dhcpv4 'server'
        option ra_slaac '1'
        list ra_flags 'managed-config'
        list ra_flags 'other-config'
        option ra 'relay'
        option dhcpv6 'relay'
        option ndp 'relay'
        list dns '2001:4860:4860::8888'
        list dns '2001:4860:4860::8844'

config dhcp 'wan'
        option interface 'wan'
        option ignore '1'
        option master '1'
        option ra 'relay'
        option dhcpv6 'relay'
        option ndp 'relay'
```

## DDNS设置

### Cloudflare
+ 登陆Cloudflare——选择对应的域名——点击DNS——添加记录——类型选择AAAA——记录值填上对应的IPV6地址
+ 获取API令牌：点击概览——点击获取您的API令牌——选择Global API Key——点击查看
+ 在`路由器`-`服务`-`动态DNS`下添加一条记录：
   1.	DDNS服务提供商：cloudflare.com-v4 点击更改提供者
   2.	勾选启用
   3.	查询主机名：主机记录.一级域名
   4.	IP地址版本： IPv6 地址
   5.	域名：主机记录.一级域名
   6.	用户名：填写cloudflare账号 #注意检查前面是否有空格
   7.	密码：Global API Key  #注意检查前面是否有空格
   8.	高级设置——网络（IPV6）——选择LAN口
   9.	保存&设置

+ 路由器管理界面设置IPV6端口转发，使用`网络`-`Socat`

### 卸载插件
```
opkg list_installed | grep xteve
opkg remove luci-app-xteve - git-25.054.68431-4e7d809
```