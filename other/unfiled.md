# 未分类
## Jenkins
- 插件下载地址
  - [hpi插件地址](http://updates.jenkins-ci.org/latest/)
  - [Theme插件](http://wiki.jenkins-ci.org/display/JENKINS/Simple+Theme+Plugin)
- 页面自定义
  - 修改jar包`jenkins/WEB-INF/lib/jenkins-core-1.651.3.jar`文件中的`lib/layout/layout.jelly`

## maven安装本地jar
```
mvn install:install-file -Dfile=junit-4.8.1.jar -DgroupId=junit -DartifactId=junit -Dversion=4.8.1 -Dpackaging=jar
```