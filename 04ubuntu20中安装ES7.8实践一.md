# ubuntu20中安装ES7.8

- [ubuntu20中安装ES7.8](#ubuntu20中安装es78)
  - [在ubuntu20 中安装jdk8](#在ubuntu20-中安装jdk8)
  - [在线安装openjdk8。而oracle Java JDK(Ubuntu20.04实测不行，就不记录了)](#在线安装openjdk8而oracle-java-jdkubuntu2004实测不行就不记录了)
  - [安装Elasticsearch](#安装elasticsearch)
  - [elasticsearch-analysis-ik 安装ik分词插件](#elasticsearch-analysis-ik-安装ik分词插件)
  - [安装Kibana 分析和可视化](#安装kibana-分析和可视化)


## 在ubuntu20 中安装jdk8

[linux常用命令入门参考地址](https://www.showdoc.com.cn/p/874d965ad7a05931e3a1ede278d99a64)

1,先查询电脑cpu

```shell
还可以使用命令“uname -a”查看，
输出的结果中，如果有x86_64就是64位(X64)，没有就是32位(X86)。

可以用命令“getconf LONG_BIT”查看，
如果返回的结果是32则说明是32位(X86)，返回的结果是64则说明是64位(X64)。

```

2,查询ubuntu 的版本及内核,选择 jdk-8u301-linux-x64.tar.gz 安装。

```shell
// 1.查看内核版本
zengdx@ubuntu:~$ cat /proc/version
Linux version 3.19.0-25-generic (buildd@lgw01-20) (gcc version 4.8.2 (Ubuntu 4.8.2-19ubuntu1) ) #26~14.04.1-Ubuntu SMP Fri Jul 24 21:16:20 UTC 2015

zengdx@ubuntu:~$ uname -a
Linux ubuntu 3.19.0-25-generic #26~14.04.1-Ubuntu SMP Fri Jul 24 21:16:20 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux

zengdx@ubuntu:~$ uname -r
3.19.0-25-generic

//2.查看系统版本
zengdx@ubuntu:~$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 14.04.3 LTS
Release:        14.04
Codename:       trusty

zengdx@ubuntu:~$ cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=14.04
DISTRIB_CODENAME=trusty
DISTRIB_DESCRIPTION="Ubuntu 14.04.3 LTS"
zengdx@ubuntu:~$ cat /etc/issue
Ubuntu 14.04.3 LTS \n \l

```

3,上传jdk包，并解压

+ ubuntu服务器上 创建接收上传jdk和解压时的文件夹jvm
 sudo mkdir /usr/lib/jvm

+ 上传时报状态错误，解决办法就是修改新建文件的 权限

在jvm 的上一级目录中 执行 sudo chmod 777 jvm
```shell
➜  lib chmod 777 jvm   
chmod: 正在更改 'jvm' 的权限: 不允许的操作
➜  lib sudo chmod 777 jvm   
[sudo] backend 的密码： 
➜  lib cd jvm       
➜  jvm 

```
或者
<div align='center'><img src=./images/04ubuntu20中安装ES7.8实践一/04ubuntu20中安装ES7.8实践一_2021-08-21-11-55-15.png width='100%'/></div><br/>

右键 -- 更改权限 -- 全部都勾上（如下图）
<div align='center'><img src=./images/04ubuntu20中安装ES7.8实践一/04ubuntu20中安装ES7.8实践一_2021-08-21-11-55-45.png width='100%'/></div><br/>

+ 点开jvm文件夹，直接将左边要上传的jdk-8u301-linux-x64.tar.gz包 拖到右边 jvm 文件夹中

+ 解压上传的文件

```shell
#jdk8
sudo tar -zxvf jdk-8u301-linux-x64.tar.gz -C /usr/lib/jvm
#jdk11
sudo tar -zxvf jdk-11.0.12_linux-x64_bin.tar.gz -C /usr/lib/jvm
```


+ 修改环境变量
`sudo vim ~/.bashrc`

```shell
vim 命令说明： 
输入 i =>  插入 
按 ESC 退出编辑
输入 :wq + enter => 退出vim 编辑并保存
```

+ 文件末尾追加如下内容

```shell
# jdk-11.0.12
# export JAVA_HOME=/usr/lib/jvm/jdk-11.0.12

# set oracle jdk8 environment
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_301
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH  
```

+ 使环境变量生效
  
`source ~/.bashrc`

以上修改环境变量,重新加载环境配置时可能会报错

```shell
~ source ~/.bashrc
/home/backend/.bashrc:16: command not found: shopt
/home/backend/.bashrc:24: command not found: shopt
/home/backend/.bashrc:111: command not found: shopt
/usr/share/bash-completion/bash_completion:45: command not found: shopt
/usr/share/bash-completion/bash_completion:1512: parse error near `|'
\[\e]0;\u@\h: \w\a\]\u@\h:\w$ 

```

+ 如果上面报错，我们将java 的环境变量配置在.zshrc 中

`sudo vim ~/.zshrc`

```shell
# jdk-11.0.12
# export JAVA_HOME=/usr/lib/jvm/jdk-11.0.12

#set oracle jdk environment
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_301
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH  
```

 <div align='center'><img src=./images/04ubuntu20中安装ES7.8实践一/04ubuntu20中安装ES7.8实践一_2021-08-21-15-05-42.png width='100%'/></div><br/>

重新加载环境配置，使修改的环境生效
 `source ~/.zshrc`

+ 设置默认jdk

```shell
#jdk8
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_301/bin/java 300  
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_301/bin/javac 300  
sudo update-alternatives --install /usr/bin/jar jar /usr/lib/jvm/jdk1.8.0_301/bin/jar 300   
sudo update-alternatives --install /usr/bin/javah javah /usr/lib/jvm/jdk1.8.0_301/bin/javah 300   
sudo update-alternatives --install /usr/bin/javap javap /usr/lib/jvm/jdk1.8.0_301/bin/javap 300

#jdk11
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk-11.0.12/bin/java 300  
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk-11.0.12/bin/javac 300  
sudo update-alternatives --install /usr/bin/jar jar /usr/lib/jvm/jdk-11.0.12/bin/jar 300   
sudo update-alternatives --install /usr/bin/javah javah /usr/lib/jvm/jdk-11.0.12/bin/javah 300   
sudo update-alternatives --install /usr/bin/javap javap /usr/lib/jvm/jdk-11.0.12/bin/javap 300 

```

+ 执行

```shell
#前面带有 * 号的表示当前有效路径,可以选择编号来选择有效路径
sudo update-alternatives --config java
```



+ 测试是否安装成功

`java -version `

## 在线安装openjdk8。而oracle Java JDK(Ubuntu20.04实测不行，就不记录了)

+ 更新软件包列表：
  
` sudo apt-get update `

+ 安装openjdk-8-jdk：
  
`sudo apt-get install openjdk-8-jdk `

+ 查看java版本，看看是否安装成功：

`java -version`


//Ubuntu 20.04.2.0 LTS 系统安装过程详解 （从下载镜像到安装系统）

https://blog.csdn.net/weixin_39278265/article/details/117594161

//这里是安装Elasticsearch的参考博客

https://blog.csdn.net/shangshu0707/article/details/89702622?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-2.essearch_pc_relevant&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-2.essearch_pc_relevant


https://www.cnblogs.com/chenqionghe/p/10425751.html

https://blog.csdn.net/u011863024/article/details/115721328

https://www.jianshu.com/p/f239520f21f8


es启动报错解决1
https://www.cnblogs.com/biehongli/p/15136408.html

## 安装Elasticsearch


1, 新建目录和解压安装包，创建组及用户

```shell
// 新建目录 install用于存放待解压的安装包
sudo mkdir /usr/local/install
#进入 local 文件夹执行： 
sudo chmod 777 install

//新建存放解压包的目录
sudo mkdir /usr/local/elasticsearch
#进入 local 文件夹执行：
sudo chmod 777 elasticsearch

#解包到 指定文件夹 /usr/local/elasticsearch
sudo tar -zxvf elasticsearch-7.8.0-linux-x86_64.tar.gz -C /usr/local/elasticsearch/

#改名成 es
sudo mv elasticsearch-7.8.0 es

#创建用户组 es
sudo addgroup es

#创建用户 es_user
sudo adduser es_user
#输入新密码：es_user

#用户添加到 es 组
sudo usermod -g es es_user

#为该用户添加管理员权限(vim /etc/sudoers也可以)，如下图
sudo visudo
```

<div align='center'><img src=./images/04ubuntu20中安装ES7.8实践一/04ubuntu20中安装ES7.8实践一_2021-08-23-12-48-52.png width='100%'/></div><br/>


```shell
#让 es_user 用户拥有对 elasticsearch 的执行权限
sudo chown -R es_user:es /usr/local/elasticsearch/

#切换用户
su es_user
#需要输入之前设置的密码：es_user

#进入config目录
#cd /usr/local/elasticsearch/es/config
es_user@backend-desktop:/$ cd /usr/local/elasticsearch/es/config
es_user@backend-desktop:/usr/local/elasticsearch/es/config$ 

#备份配置文件
#config目录下执行下列命令
cp elasticsearch.yml elasticsearch.yml.bak
```

2, **下列修改配置文件都需要在root权限下或者sudo命令下**

```shell
#修改配置文件，添加如下内容
#config目录下执行下列命令
cd /usr/local/elasticsearch/es/config
sudo vim elasticsearch.yml
```

用vim 命令在配置文件 elasticsearch.yml 中添加以下配置

```shell
#加入如下配置
cluster.name: elasticsearch
node.name: node-1
network.host: 0.0.0.0
http.port: 9200
cluster.initial_master_nodes: ["node-1"]

// 下面对上面配置的注释说明
#集群name
cluster.name: elasticsearch
#节点name
node.name: node-1
#端口
http.port: 9200
#地址
network.host: 0.0.0.0
#引导启动集群
cluster.initial_master_nodes: ["node-1"]
```

修改/etc/security/limits.conf  => sudo vim /etc/security/limits.conf
```shell
#在文件末尾中增加下面内容
#es_user用户下每个进程可以打开的文件数的限制
es_user soft nofile 65536
es_user hard nofile 65536
```

修改/etc/security/limits.d/20-nproc.conf  => sudo vim /etc/security/limits.d/20-nproc.conf 
```shell
#在文件末尾中增加下面内容
#es_user用户下每个进程可以打开的文件数的限制
es_user soft nofile 65536
es_user hard nofile 65536
#操作系统级别对每个用户创建的进程数的限制
* hard nproc 4096
#注： * 带表 Linux 所有用户名称

```

修改/etc/sysctl.conf   => sudo vim /etc/sysctl.conf
```shell
#在文件中增加下面内容
#一个进程可以拥有的 VMA(虚拟内存区域)的数量,默认值为 65536
vm.max_map_count=655360
```

重新加载
```shell
sudo sysctl -p
```

3, 使用es_user用户启动elasticsearch

```shell
#切换用户
su es_user
#需要输入之前设置的密码：es_user

cd /usr/local/elasticsearch/es/
#启动
bin/elasticsearch
#后台启动
bin/elasticsearch -d 
```

3.1, 启动时报错汇总

+ 如果启动时报以下的错误
<div align='center'><img src=./images/04ubuntu20中安装ES7.8实践一/04ubuntu20中安装ES7.8实践一_2021-08-23-15-08-06.png width='100%'/></div><br/>

> 报错原因说明：启动时，会动态生成文件，如果文件所属用户不匹配，会发生错误，需要重新进行修改用户和用户组
> 这时需要从新执行一下，让用户拥有操作生成文件的权限

```shell
sudo chown -R es_user:es /usr/local/elasticsearch/es/
```

+ 后端已经启动，重复启动时报错1：

```shell
[2021-08-25T17:00:38,255][ERROR][o.e.b.ElasticsearchUncaughtExceptionHandler] [node-1] uncaught exception in thread [main]
org.elasticsearch.bootstrap.StartupException: java.lang.IllegalStateException: failed to obtain node locks, tried [[/usr/local/elasticsearch/es/data]] with lock id [0]; maybe these locations are not writable or multiple nodes were started without increasing [node.max_local_storage_nodes] (was [1])?
	at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:174) ~[elasticsearch-7.8.0.jar:7.8.0]
	at org.elasticsearch.bootstrap.Elasticsearch.execute(Elasticsearch.java:161) ~[elasticsearch-7.8.0.jar:7.8.0]
	at org.elasticsearch.cli.EnvironmentAwareCommand.execute(EnvironmentAwareCommand.java:86) ~[elasticsearch-7.8.0.jar:7.8.0]
	at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:127) ~[elasticsearch-cli-7.8.0.jar:7.8.0]
	at org.elasticsearch.cli.Command.main(Command.java:90) ~[elasticsearch-cli-7.8.0.jar:7.8.0]
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:126) ~[elasticsearch-7.8.0.jar:7.8.0]
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:92) ~[elasticsearch-7.8.0.jar:7.8.0]
Caused by: java.lang.IllegalStateException: failed to obtain node locks, tried [[/usr/local/elasticsearch/es/data]] with lock id [0]; maybe these locations are not writable or multiple nodes were started without increasing [node.max_local_storage_nodes] (was [1])?

```
解决方案：
>  场景描述,后端已经启动，重复启动时，或者 在启动elastichsearch之前使用了root账号进行启动，启动失败之后，直接使用elsearch自己创建得这个账号启动es，发现报错

>解决方法是将es安装目录下面得data目录下面的nodes全部删除，重新使用elsearch启动es即可。

+ 后端已经启动，重复启动时报错2：解决错误1将报这个错误 9200端口号被占用错误

```shell
[2021-08-25T17:13:31,823][ERROR][o.e.b.ElasticsearchUncaughtExceptionHandler] [node-1] uncaught exception in thread [main]
org.elasticsearch.bootstrap.StartupException: BindHttpException[Failed to bind to 0.0.0.0:9200]; nested: BindException[地址已在使用];
	at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:174) ~[elasticsearch-7.8.0.jar:7.8.0]
	at org.elasticsearch.bootstrap.Elasticsearch.execute(Elasticsearch.java:161) ~[elasticsearch-7.8.0.jar:7.8.0]
	at org.elasticsearch.cli.EnvironmentAwareCommand.execute(EnvironmentAwareCommand.java:86) ~[elasticsearch-7.8.0.jar:7.8.0]
	at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:127) ~[elasticsearch-cli-7.8.0.jar:7.8.0]
	at org.elasticsearch.cli.Command.main(Command.java:90) ~[elasticsearch-cli-7.8.0.jar:7.8.0]
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:126) ~[elasticsearch-7.8.0.jar:7.8.0]
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:92) ~[elasticsearch-7.8.0.jar:7.8.0]
Caused by: org.elasticsearch.http.BindHttpException: Failed to bind to 0.0.0.0:9200

```

解决办法（如果一定要重新启动的话）：

```shell
#查看占用端口的进程号，需要用sudo升级权限执行。
sudo netstat -anp |grep 9200 
tcp6       0      0 :::9200                 :::*                    LISTEN      265304/java
#查看进程号的详细信息
➜  es ps -aux|grep 265304             
es_user   265304  0.8  8.3 4646184 1351976 ?     Sl   14:39   1:03 /usr/lib/jvm/jdk1.8.0_301/bin/java -Xshare:auto -Des.networkaddress.cache.ttl=60 -Des.networkaddress.cache.negative.ttl=10 -XX:+AlwaysPreTouch -Xss1m -Djava.awt.headless=true -Dfile.encoding=UTF-8 -Djna.nosys=true -XX:-OmitStackTraceInFastThrow -Dio.netty.noUnsafe=true -Dio.netty.noKeySetOptimization=true -Dio.netty.recycler.maxCapacityPerThread=0 -Dio.netty.allocator.numDirectArenas=0 -Dlog4j.shutdownHookEnabled=false -Dlog4j2.disable.jmx=true -Djava.locale.providers=SPI,JRE -Xms1g -Xmx1g -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=75 -XX:+UseCMSInitiatingOccupancyOnly -Djava.io.tmpdir=/tmp/elasticsearch-5897008241081191494 -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=data -XX:ErrorFile=logs/hs_err_pid%p.log -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+PrintTenuringDistribution -XX:+PrintGCApplicationStoppedTime -Xloggc:logs/gc.log -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=32 -XX:GCLogFileSize=64m -XX:MaxDirectMemorySize=536870912 -Des.path.home=/usr/local/elasticsearch/es -Des.path.conf=/usr/local/elasticsearch/es/config -Des.distribution.flavor=default -Des.distribution.type=tar -Des.bundled_jdk=true -cp /usr/local/elasticsearch/es/lib/* org.elasticsearch.bootstrap.Elasticsearch -d
backend   272357  0.0  0.0  17688  2700 pts/4    S+   16:42   0:00 grep --color=auto --exclude-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn --exclude-dir=.idea --exclude-dir=.tox 265304
#杀掉这个进程
➜  es sudo kill -9 265304

#再重新启动es
```

启动成功是这样的
```shell
es_user@backend-desktop:/usr/local/elasticsearch/es$ bin/elasticsearch
future versions of Elasticsearch will require Java 11; your Java version from [/usr/lib/jvm/jdk1.8.0_301/jre] does not meet this requirement
future versions of Elasticsearch will require Java 11; your Java version from [/usr/lib/jvm/jdk1.8.0_301/jre] does not meet this requirement
[2021-08-23T15:51:59,952][INFO ][o.e.n.Node               ] [node-1] version[7.8.0], pid[202505], build[default/tar/757314695644ea9a1dc2fecd26d1a43856725e65/2020-06-14T19:35:50.234439Z], OS[Linux/5.8.0-63-generic/amd64], JVM[Oracle Corporation/Java HotSpot(TM) 64-Bit Server VM/1.8.0_301/25.301-b09]
[2021-08-23T15:51:59,955][INFO ][o.e.n.Node               ] [node-1] JVM home [/usr/lib/jvm/jdk1.8.0_301/jre]
[2021-08-23T15:51:59,955][INFO ][o.e.n.Node               ] [node-1] JVM arguments [-Xshare:auto, -Des.networkaddress.cache.ttl=60, -Des.networkaddress.cache.negative.ttl=10, -XX:+AlwaysPreTouch, -Xss1m, -Djava.awt.headless=true, -Dfile.encoding=UTF-8, -Djna.nosys=true, -XX:-OmitStackTraceInFastThrow, -Dio.netty.noUnsafe=true, -Dio.netty.noKeySetOptimization=true, -Dio.netty.recycler.maxCapacityPerThread=0, -Dio.netty.allocator.numDirectArenas=0, -Dlog4j.shutdownHookEnabled=false, -Dlog4j2.disable.jmx=true, -Djava.locale.providers=SPI,JRE, -Xms1g, -Xmx1g, -XX:+UseConcMarkSweepGC, -XX:CMSInitiatingOccupancyFraction=75, -XX:+UseCMSInitiatingOccupancyOnly, -Djava.io.tmpdir=/tmp/elasticsearch-9190630980316763672, -XX:+HeapDumpOnOutOfMemoryError, -XX:HeapDumpPath=data, -XX:ErrorFile=logs/hs_err_pid%p.log, -XX:+PrintGCDetails, -XX:+PrintGCDateStamps, -XX:+PrintTenuringDistribution, -XX:+PrintGCApplicationStoppedTime, -Xloggc:logs/gc.log, -XX:+UseGCLogFileRotation, -XX:NumberOfGCLogFiles=32, -XX:GCLogFileSize=64m, -XX:MaxDirectMemorySize=536870912, -Des.path.home=/usr/local/elasticsearch/es, -Des.path.conf=/usr/local/elasticsearch/es/config, -Des.distribution.flavor=default, -Des.distribution.type=tar, -Des.bundled_jdk=true]
[2021-08-23T15:52:03,232][INFO ][o.e.p.PluginsService     ] [node-1] loaded module [aggs-matrix-stats]
[2021-08-23T15:52:03,232][INFO ][o.e.p.PluginsService     ] [node-1] loaded module [analysis-common]
...
[2021-08-23T15:52:03,245][INFO ][o.e.p.PluginsService     ] [node-1] loaded module [x-pack-watcher]
[2021-08-23T15:52:03,246][INFO ][o.e.p.PluginsService     ] [node-1] no plugins loaded
[2021-08-23T15:52:03,303][INFO ][o.e.e.NodeEnvironment    ] [node-1] using [1] data paths, mounts [[/ (/dev/sda2)]], net usable_space [96.6gb], net total_space [116.3gb], types [ext4]
[2021-08-23T15:52:03,304][INFO ][o.e.e.NodeEnvironment    ] [node-1] heap size [990.7mb], compressed ordinary object pointers [true]
[2021-08-23T15:52:12,322][INFO ][o.e.c.c.Coordinator      ] [node-1] setting initial configuration to VotingConfiguration{ZoU6tj_nSEGULsV87ll0yw}
[2021-08-23T15:52:12,533][INFO ][o.e.c.s.MasterService    ] [node-1] elected-as-master ([1] nodes joined)[{node-1}{ZoU6tj_nSEGULsV87ll0yw}{hu4WTc16RpCs9TTp5sGowg}{10.9.11.38}{10.9.11.38:9300}{dilmrt}{ml.machine_memory=16679190528, xpack.installed=true, transform.node=true, ml.max_open_jobs=20} elect leader, _BECOME_MASTER_TASK_, _FINISH_ELECTION_], term: 1, version: 1, delta: master node changed {previous [], current [{node-1}{ZoU6tj_nSEGULsV87ll0yw}{hu4WTc16RpCs9TTp5sGowg}{10.9.11.38}{10.9.11.38:9300}{dilmrt}{ml.machine_memory=16679190528, xpack.installed=true, transform.node=true, ml.max_open_jobs=20}]}
[2021-08-23T15:52:12,577][INFO ][o.e.c.c.CoordinationState] [node-1] cluster UUID set to [yMVfntnoRpO1hhhzTH5d4g]
[2021-08-23T15:52:12,608][INFO ][o.e.c.s.ClusterApplierService] [node-1] master node changed {previous [], current [{node-1}{ZoU6tj_nSEGULsV87ll0yw}{hu4WTc16RpCs9TTp5sGowg}{10.9.11.38}{10.9.11.38:9300}{dilmrt}{ml.machine_memory=16679190528, xpack.installed=true, transform.node=true, ml.max_open_jobs=20}]}, term: 1, version: 1, reason: Publication{term=1, version=1}
[2021-08-23T15:52:12,671][INFO ][o.e.h.AbstractHttpServerTransport] [node-1] publish_address {10.9.11.38:9200}, bound_addresses {[::]:9200}
[2021-08-23T15:52:12,672][INFO ][o.e.n.Node               ] [node-1] started
[2021-08-23T15:52:12,693][INFO ][o.e.x.c.t.IndexTemplateRegistry] [node-1] adding index template [.watch-history-11] for [watcher], because it doesn't exist
[2021-08-23T15:52:12,694][INFO ][o.e.x.c.t.IndexTemplateRegistry] [node-1] adding index template [.triggered_watches] for [watcher], because it doesn't exist
...
[2021-08-23T15:52:12,747][INFO ][o.e.x.c.t.IndexTemplateRegistry] [node-1] adding index template [ilm-history] for [index_lifecycle], because it doesn't exist
[2021-08-23T15:52:12,750][INFO ][o.e.x.c.t.IndexTemplateRegistry] [node-1] adding index template [.slm-history] for [index_lifecycle], because it doesn't exist
[2021-08-23T15:52:12,804][INFO ][o.e.g.GatewayService     ] [node-1] recovered [0] indices into cluster_state
[2021-08-23T15:52:13,184][INFO ][o.e.c.m.MetadataIndexTemplateService] [node-1] adding template [.watch-history-11] for index patterns [.watcher-history-11*]
[2021-08-23T15:52:13,230][INFO ][o.e.c.m.MetadataIndexTemplateService] [node-1] adding template [.watches] for index patterns [.watches*]
[2021-08-23T15:52:13,272][INFO ][o.e.c.m.MetadataIndexTemplateService] [node-1] adding template [.monitoring-kibana] for index patterns [.monitoring-kibana-7-*]
[2021-08-23T15:52:13,980][INFO ][o.e.x.i.a.TransportPutLifecycleAction] [node-1] adding index lifecycle policy [watch-history-ilm-policy]
[2021-08-23T15:52:14,028][INFO ][o.e.x.i.a.TransportPutLifecycleAction] [node-1] adding index lifecycle policy [ml-size-based-ilm-policy]
[2021-08-23T15:52:14,073][INFO ][o.e.x.i.a.TransportPutLifecycleAction] [node-1] adding index lifecycle policy [ilm-history-ilm-policy]
[2021-08-23T15:52:14,106][INFO ][o.e.x.i.a.TransportPutLifecycleAction] [node-1] adding index lifecycle policy [slm-history-ilm-policy]
[2021-08-23T15:52:14,277][INFO ][o.e.l.LicenseService     ] [node-1] license [2fc705d4-6257-4525-87d5-7ea98716a88b] mode [basic] - valid
[2021-08-23T15:52:14,279][INFO ][o.e.x.s.s.SecurityStatusChangeListener] [node-1] Active license is now [BASIC]; Security is disabled

```

4, 关闭防火墙
```shell
#暂时关闭防火墙
systemctl stop firewalld
#永久关闭防火墙
systemctl enable firewalld.service #打开防火墙永久性生效，重启后不会复原
systemctl disable firewalld.service #关闭防火墙，永久性生效，重启后不会复原

```

5, 测试软件
浏览器中输入地址： http://ip:9200/

<div align='center'><img src=./images/04ubuntu20中安装ES7.8实践一/04ubuntu20中安装ES7.8实践一_2021-08-23-16-01-56.png width='100%'/></div><br/>

6,关闭elasticsearch

```shell
#查看elastic的进程号 并杀死
ps aux | grep elasticsearch
kill -9 进程号

#查看elastic的进程号 并杀死
ps -ef | grep elastic
kill -9 2382（进程号）

#重启 -d 后台运行
./bin/elasticsearch -d

```
## elasticsearch-analysis-ik 安装ik分词插件

+ [下载地址-注意版本的对应](https://github.com/medcl/elasticsearch-analysis-ik/releases/tag/v7.8.0
)

`https://github.com/medcl/elasticsearch-analysis-ik/releases/tag/v7.8.0`
>由于 ElasticSearch 默认的分词器不支持中文分词，所以我们需要集成ik 分词器。

>找到对应版本，下载解压到Elasticsearch的/plugins/ik目录下即可（版本一定要与Elasticsearch版本一致）。

+ 必须在plugins文件夹下新建ik或者analysis-ik文件夹，再解压到这新建的 **ik** 或者**analysis-ik**文件夹下。
如果elasticsearch-analysis-ik-7.8.0.zip版本，直接解压至plugins文件夹后启动es时报错如下图：

<div align='center'><img src=./images/04ubuntu20中安装ES7.8实践一/04ubuntu20中安装ES7.8实践一_2021-08-25-12-36-52.png width='100%'/></div><br/>

原因分析：

刚开始看见某个文件找不到，查看了config文件夹确实没有文件，以为是插件版本不一致的问题。使用./bin/elasticsearch-plugins install elasticsearch-analysis-ik-7.8.0.zip也报插件找不到。

解决方法：

解压elasticsearch-analysis-ik-7.8.0.zip到 /plugins/ik 文件夹下，ik文件夹名字是必须的，因为elasticsearch安装的插件名称，重启elasticsearch一切OK。

+ 上传到服务器然后解压到指定文件夹,必须在plugins文件夹下新建ik或者analysis-ik文件夹

```shell
#在es的plugins文件夹下，创建ik文件夹，再给ik文件夹设置777权限即读写权限都有
sudo mkdir ik
sudo chmod 777 ik

#解包到 上传到指定文件夹 /usr/local/elasticsearch/es/plugins/ik
#解压到当前文件夹
sudo unzip elasticsearch-analysis-ik-7.8.0.zip

#回到上一级目录ik
cd ..

#查看plugins/ik文件夹下的内容
ls

#然后在es/plugins目录下执行 tree ik
sudo tree ik/

#如果解压好了，则删除压缩包
#在ik目录下
sudo rm -rf elasticsearch-analysis-ik-7.8.0.zip
```

报错内容`zsh: command not found: tree`

解决办法：

```shell
1、vi .bash_profile 
在.bash_profile 中添加一行： 
export PATH=/bin:/usr/bin:/usr/local/bin:$PATH

2、vim .zshrc 
在.zshrc中添加一行： 
source ~/.bash_profile

3，重写加载环境配置
source ~/.zshrc
```

安装tree命令的依赖包 执行命令`sudo apt-get install tree`

错误提示：
```shell
E: 无法获得锁 /var/lib/dpkg/lock-frontend。锁正由进程 239002（apt-get）持有
N: 请注意，直接移除锁文件不一定是合适的解决方案，且可能损坏您的系统。
E: 无法获取 dpkg 前端锁 (/var/lib/dpkg/lock-frontend)，是否有其他进程正占用它？
```
解决办法：
强制删除，命令行如下
```shell
sudo rm /var/lib/dpkg/lock-frontend
sudo rm /var/lib/dpkg/lock
```
再重写执行 `sudo apt-get install tree`

再查看我们的解压包情况 `sudo tree ik` 执行命令及展示结果如下：

```shell
➜  / cd /usr/local/elasticsearch/es/plugins
➜  plugins sudo tree ik/
ik/
├── commons-codec-1.9.jar
├── commons-logging-1.2.jar
├── config
│   ├── extra_main.dic
│   ├── extra_single_word.dic
│   ├── extra_single_word_full.dic
│   ├── extra_single_word_low_freq.dic
│   ├── extra_stopword.dic
│   ├── IKAnalyzer.cfg.xml
│   ├── main.dic
│   ├── preposition.dic
│   ├── quantifier.dic
│   ├── stopword.dic
│   ├── suffix.dic
│   └── surname.dic
├── elasticsearch-analysis-ik-7.8.0.jar
├── httpclient-4.5.2.jar
├── httpcore-4.4.4.jar
├── plugin-descriptor.properties
└── plugin-security.policy

1 directory, 19 files
```

## 安装Kibana 分析和可视化

Kibana是一个针对Elasticsearch的开源分析及可视化平台，用来搜索、查看交互存储在Elasticsearch索引中的数据
选择对应自己Elasticsearch版本和操作系统的Kibana安装包，笔者选择的7.8.0的tar包

+ [Kibana官网下载地址](https://www.elastic.co/cn/downloads/kibana)

+ 上传，解压安装，修改配置

```shell
#新建目录
sudo mkdir /usr/local/kibana

#解包
sudo tar -zxvf kibana-7.8.0-linux-x86_64.tar.gz -C /usr/local/kibana/

#先切换到管理员账号，赋予es_user用户权限
sudo chown -R es_user:es /usr/local/kibana/

#进入/config/目录修改配置文件
sudo cp kibana.yml kibana.yml.bak
sudo vim kibana.yml 

#添加以下内容,如何es安装在同一台服务器ip可以用localhost
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://localhost:9200"]

```

+ 启动Kibana(需要先后台启动es)

```shell
#进入/bin/目录，启动kibana
 cd ..
 cd bin/
 ./kibana
 
 #通过 ./kibana 启动时， 关闭kibana 用
 ctrl+C

 #后台启动：
 ./kibana &

#访问5601端口，出现下图即安装成功
http:/ip:5601/
```

<div align='center'><img src=./images/04ubuntu20中安装ES7.8实践一/04ubuntu20中安装ES7.8实践一_2021-08-24-18-12-36.png width='100%'/></div><br/>

+ 给Kibana 设置中文

Kibana 7.x 官方支持中文，只需要修改 kibana.yml 即可

```shell
#在/config/kibana.yml文件中添加
i18n.locale: "zh-CN"
```

修改后重新启动kibana即可

至于其他版本，可以去下载补丁包手动汉化，此处不再赘述。

+ 启动kibana时报错

```shell
  log   [09:52:36.732] [warning][savedobjects-service] Unable to connect to Elasticsearch. Error: Request Timeout after 30000ms
  log   [09:52:39.240] [warning][savedobjects-service] Unable to connect to Elasticsearch. Error: [resource_already_exists_exception] index [.kibana_task_manager_1/k_d_-G9bS2m8LPqWuu2Znw] already exists, with { index_uuid="k_d_-G9bS2m8LPqWuu2Znw" & index=".kibana_task_manager_1" }
  log   [09:52:39.242] [warning][savedobjects-service] Another Kibana instance appears to be migrating the index. Waiting for that migration to complete. If no other Kibana instance is attempting migrations, you can get past this message by deleting index .kibana_task_manager_1 and restarting Kibana.
  log   [09:52:39.249] [warning][savedobjects-service] Unable to connect to Elasticsearch. Error: [resource_already_exists_exception] index [.kibana_1/ZebhsjZ8SwqJJ3bZVyC-Zw] already exists, with { index_uuid="ZebhsjZ8SwqJJ3bZVyC-Zw" & index=".kibana_1" }
  log   [09:52:39.250] [warning][savedobjects-service] Another Kibana instance appears to be migrating the index. Waiting for that migration to complete. If no other Kibana instance is attempting migrations, you can get past this message by deleting index .kibana_1 and restarting Kibana.
```
> 这已经很明了了，you can get past this message by deleting index .kibana_1 and restarting Kibana。

>查看es的索引，果然发现有.kibana_1这个索引，那么我们就删除该索引。（暂不知道如何查看）

>新搭建es常会有.kibana_1索引

在启动的es中执行`curl -XDELETE http://10.9.11.38:9200/.kibana*` 如下：

```shell
es_user@backend-desktop:/usr/local/elasticsearch$ curl -XDELETE http://10.9.11.38:9200/.kibana*
{"acknowledged":true}es_user@backend-desktop:/usr/local/elasticsearch$ 
```

然后停掉es,再重新启动es（具体操作参照上面的es）

最后再次启动kibana, 后台启动：`./kibana &`

```shell
es_user@backend-desktop:/usr/local/kibana/kibana-7.8.0-linux-x86_64/bin$ ./kibana &
  log   [10:18:07.438] [warning][plugins-discovery] Expect plugin "id" in camelCase, but found: apm_oss
  log   [10:18:07.455] [warning][plugins-discovery] Expect plugin "id" in camelCase, but found: triggers_actions_ui
  log   [10:18:14.436] [info][plugins-service] Plugin "visTypeXy" is disabled.
  log   [10:18:14.437] [info][plugins-service] Plugin "endpoint" is disabled.
  log   [10:18:14.437] [info][plugins-service] Plugin "ingestManager" is disabled.
  log   [10:18:14.438] [info][plugins-service] Plugin "lists" is disabled.
  ...
  log   [10:18:22.835] [info][status][plugin:console_legacy@7.8.0] Status changed from uninitialized to green - Ready
  log   [10:18:22.841] [info][status][plugin:region_map@7.8.0] Status changed from uninitialized to green - Ready
  log   [10:18:22.853] [info][status][plugin:ui_metric@7.8.0] Status changed from uninitialized to green - Ready
  log   [10:18:22.868] [info][listening] Server running at http://0.0.0.0:5601
  log   [10:18:23.538] [info][server][Kibana][http] http server running at http://0.0.0.0:5601

```

+ 浏览器访问`http://10.9.11.38:5601/`
  
  <div align='center'><img src=./images/04ubuntu20中安装ES7.8实践一/04ubuntu20中安装ES7.8实践一_2021-08-25-18-42-56.png width='100%'/></div><br/>

+ 关闭后台启动的 kibana

```shell
#查看elastic的进程号 并杀死
ps -ef | grep kibana
kill -9 279265（进程号）
```
