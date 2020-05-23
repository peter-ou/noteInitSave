# 04云服务器上安装jdk实践一

>使用阿里云服务器ECS通过yum、安装与卸载jdk 为例。

- [04云服务器上安装jdk实践一](#04云服务器上安装jdk实践一)
  - [查询是否已经安装了jdk](#查询是否已经安装了jdk)

## 查询是否已经安装了jdk

1. linux服务器中常用命令

    ```linux
    1，查询根目录下的所有目录
    [root@izwz9dc94s2y45exdo7dy0z ~]# ls /
    2，回到根目录
    [root@izwz9dc94s2y45exdo7dy0z ~]# cd /
    3，查看当前目录下的所有子目录
    [root@izwz9dc94s2y45exdo7dy0z /]# ls
    4，进入某一个子目录
    [root@izwz9dc94s2y45exdo7dy0z usr]# cd /usr
    ```

2. 检查是否安装了jdk和jdk的卸载

    ```linux
    1. 查看linux中是否安装了jdk的命令： rpm -qa | grep -i java 或者 rpm -qa| grep jdk
    [root@izwz9dc94s2y45exdo7dy0z ~]# rpm -qa| grep jdk
    得到的结果为：
    java-1.8.0-openjdk-headless-1.8.0.252.b09-2.el7_8.x86_64

    2. 卸载命令为：rpm -e --nodeps 需要卸载的程序名称
    [root@izwz9dc94s2y45exdo7dy0z ~]# rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.252.b09-2.el7_8.x86_64
    结果省略了...
    ```

3. 通过yum安装jdk (yum指令安装jdk，无线配置环境变量。)

    ```linux
    1.首先更新一下YUM源      #yum -y update
    2.列出所有的JDK版本      #yum list Java*
    3.列出所有的JDK1.8版本  #yum list java-1.8*
    4.安装JDK1.8    #yum install java-1.8.0-openjdk* -y
    5.检查安装jdk的版本（验证jdk是否安装成功）  #java -version
    6.搜索jdk所有版本   #yum search java | grep jdk
    7.安装自己指定的版本 #yum install java-1.8.0-openjdk.x86_64
    ```

4. 如何查询jdk或者其他应用的安装路径
    > 首先要申明一下which java是定位不到安装路径的。which java定位到的是java程序的执行路径。

    ```linux
    [root@izwz9dc94s2y45exdo7dy0z ~]# java -version
    openjdk version "1.8.0_252"
    OpenJDK Runtime Environment (build 1.8.0_252-b09)
    OpenJDK 64-Bit Server VM (build 25.252-b09, mixed mode)
    ```

    ```linux
    [root@izwz9dc94s2y45exdo7dy0z ~]# which java
    /usr/bin/java
    ```

    ```linux
    [root@izwz9dc94s2y45exdo7dy0z ~]# ls -lrt /usr/bin/java
    lrwxrwxrwx 1 root root 22 May 23 20:04 /usr/bin/java -> /etc/alternatives/java
    ```

    ```linux
    [root@izwz9dc94s2y45exdo7dy0z ~]# ls -lrt /etc/alternatives/java
    lrwxrwxrwx 1 root root 73 May 23 20:04 /etc/alternatives/java -> /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.252.b09-2.el7_8.x86_64/jre/bin/java
    ```

    ```linux
    [root@izwz9dc94s2y45exdo7dy0z ~]# cd  /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.252.b09-2.el7_8.x86_64
    ```

    ```linux
    执行ll命令查看文件属性
    ```
