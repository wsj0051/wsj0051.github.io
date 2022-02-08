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

1. 删除windows保护历史记录，`C:\ProgramData\Microsoft\Windows Defender\Scans\History\Service\DetectionHistory`目录下所有文件夹删除
2. 新建文件夹，重命名输入`上帝模式.{ED7BA470-8E54-465E-825C-99712043E01C}`
