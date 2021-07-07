# useradd命令详解
[原文链接](https://www.runoob.com/linux/linux-comm-useradd.html)

## 语法
```
useradd [-mMnr][-c <备注>][-d <登入目录>][-e <有效期限>][-f <缓冲天数>][-g <群组>][-G <群组>][-s <shell>][-u <uid>][用户帐号]
```
或
```
useradd -D [-b][-e <有效期限>][-f <缓冲天数>][-g <群组>][-G <群组>][-s <shell>]
```
## 参数说明
- -c<备注> 　加上备注文字。备注文字会保存在passwd的备注栏位中。
- -d<登入目录> 　指定用户登入时的起始目录。
- -D 　变更预设值．
- -e<有效期限> 　指定帐号的有效期限。
- -f<缓冲天数> 　指定在密码过期后多少天即关闭该帐号。
- -g<群组> 　指定用户所属的群组。
- -G<群组> 　指定用户所属的附加群组。
- -m 　自动建立用户的登入目录。
- -M 　不要自动建立用户的登入目录。
- -n 　取消建立以用户名称为名的群组．
- -r 　建立系统帐号。
- -s<shell>　 　指定用户登入后所使用的shell。
- -u<uid> 　指定用户ID。

## 实例
添加一般用户
```
useradd tt
```
为添加的用户指定相应的用户组
```
useradd -g root tt
```
创建一个系统用户
```
useradd -r tt
```
为新添加的用户指定home目录
```
useradd -d /home/myd tt
```
建立用户且制定ID
```
useradd caojh -u 544
```
禁止用户wsj0051登录
```
 usermod -s /sbin/nologin wsj0051
```
恢复用户wsj0051登录
```
usermod -s /bin/bash wsj0051

```
其他

![笔记](https://cdn.jsdelivr.net/gh/wsj0051/files@main/pics/img/usermod.png)
