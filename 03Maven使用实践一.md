# Maven使用实践记录一

- [Maven使用实践记录一](#maven使用实践记录一)
  - [1,setting.xml的使用](#1settingxml的使用)
  - [2,常用的maven命令](#2常用的maven命令)
  - [3,将本地 jar 包安装到 Maven 仓库](#3将本地-jar-包安装到-maven-仓库)

## 1,setting.xml的使用

- 不能改成其他名字配置到idea中，这样的setting文件将是无效的;
- 查看有效的setting的命令是 `mvn help:effective-settings` ,此命令还可以查看 **当前项目的pom对应的项目的setting.xml配置的影响**,看到的为有效总配置内容信息;

## 2,常用的maven命令

```shell
# 查看有效的setting配置内容
mvn help:effective-settings
# 打包到本地仓库，跳过test模块测试的命令；
# 需要先删除本地对应的版本号
mvn clean install -Dmaven.test.skip=true
# 打包到nexus私有仓库，跳过test模块测试的命令；
# 需要先删除私有仓库对应的版本号
mvn clean deploy -Dmaven.test.skip=true
# 查看该pom对应的项目受影响的pom配置
# 即有效的依赖配置内容及mavne插件配置内容
mvn help:effective-pom -Dverbose

```

## 3,将本地 jar 包安装到 Maven 仓库

- 这里我们使用 install 插件的 install-file 目标：

```shell
mvn install:install-file -Dfile=[体系外 jar 包路径] \
-DgroupId=[给体系外 jar 包强行设定坐标] \
-DartifactId=[给体系外 jar 包强行设定坐标] \
-Dversion=1 \
-Dpackage=jar
```

- 例如（Windows 系统下使用 ^ 符号换行；Linux 系统用 \）：

```shell
mvn install:install-file -Dfile=D:\idea2019workspace\atguigu-maven-outer\out\artifacts\atguigu_maven_outer\atguigu-maven-outer.jar ^
-DgroupId=com.atguigu.maven ^
-DartifactId=atguigu-maven-outer ^
-Dversion=1 ^
-Dpackaging=jar

```
