# Nginx快速入门 - 遇见狂神说

### 公司产品出现瓶颈？

我们公司项目刚刚上线的时候，并发量小，用户使用的少，所以在低并发的情况下，一个jar包启动应用就够了，然后内部tomcat返回内容给用户。

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-21-56-22.png width='100%'/></div><br/>

但是慢慢的，使用我们平台的用户越来越多了，并发量慢慢增大了，这时候一台服务器满足不了我们的需求了。

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-21-56-40.png width='100%'/></div><br/>

于是我们横向扩展，又增加了服务器。这个时候几个项目启动在不同的服务器上，用户要访问，就需要增加一个代理服务器了，通过代理服务器来帮我们转发和处理请求。

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-21-57-15.png width='100%'/></div><br/>

我们希望这个代理服务器可以帮助我们接收用户的请求，然后将用户的请求按照规则帮我们转发到不同的服务器节点之上。这个过程用户是无感知的，用户并不知道是哪个服务器返回的结果，我们还希望他可以按照服务器的性能提供不同的权重选择。保证最佳体验！所以我们使用了Nginx。

### 什么是Nginx？

> Http代理，反向代理：作为web服务器最常用的功能之一，尤其是反向代理。

正向代理

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-21-58-20.png width='100%'/></div><br/>

反向代理

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-21-58-34.png width='100%'/></div><br/>

> Nginx提供的负载均衡策略有2种：内置策略和扩展策略。内置策略为轮询，加权轮询，Ip hash。扩展策略，就天马行空，只有你想不到的没有他做不到的。

轮询

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-21-59-06.png width='100%'/></div><br/>

加权轮询

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-21-59-22.png width='100%'/></div><br/>

iphash对客户端请求的ip进行hash操作，然后根据hash结果将同一个客户端ip的请求分发给同一台服务器进行处理，可以解决session不共享的问题。

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-22-01-14.png width='100%'/></div><br/>

> 动静分离，在我们的软件开发中，有些请求是需要后台处理的，有些请求是不需要经过后台处理的（如：css、html、jpg、js等等文件），这些不需要经过后台处理的文件称为静态文件。让动态网站里的动态网页根据一定规则把不变的资源和经常变的资源区分开来，动静资源做好了拆分以后，我们就可以根据静态资源的特点将其做缓存操作。提高资源响应的速度。

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-22-01-35.png width='100%'/></div><br/>

目前，通过使用Nginx大大提高了我们网站的响应速度，优化了用户体验，让网站的健壮性更上一层楼！

## Nginx的安装

### windows下安装

1、下载nginx

http://nginx.org/en/download.html 下载稳定版本。

以nginx/Windows-1.16.1为例，直接下载 nginx-1.16.1.zip。
下载后解压，解压后如下：

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-22-03-48.png width='100%'/></div><br/>

2、启动nginx

有很多种方法启动nginx

(1)直接双击nginx.exe，双击后一个黑色的弹窗一闪而过

(2)打开cmd命令窗口，切换到nginx解压目录下，输入命令 nginx.exe ，回车即可

3、检查nginx是否启动成功

直接在浏览器地址栏输入网址 http://localhost:80 回车，出现以下页面说明启动成功！

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-22-04-18.png width='100%'/></div><br/>

4、配置监听

nginx的配置文件是conf目录下的nginx.conf，默认配置的nginx监听的端口为80，如果80端口被占用可以修改为未被占用的端口即可。

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-22-04-33.png width='100%'/></div><br/>

当我们修改了nginx的配置文件nginx.conf 时，不需要关闭nginx后重新启动nginx，只需要执行命令 nginx -s reload 即可让改动生效

5、关闭nginx

如果使用cmd命令窗口启动nginx， 关闭cmd窗口是不能结束nginx进程的，可使用两种方法关闭nginx

(1)输入nginx命令 ` nginx -s stop `(快速停止nginx) 或 ` nginx -s quit `(完整有序的停止nginx)

(2)使用taskkill ` taskkill /f /t /im nginx.exe `

```shell
taskkill是用来终止进程的，
/f是强制终止 .
/t终止指定的进程和任何由此启动的子进程。
/im示指定的进程名称 .
```

### linux下安装

1、安装gcc

安装 nginx 需要先将官网下载的源码进行编译，编译依赖 gcc 环境，如果没有 gcc 环境，则需要安装：

```shell
yum install gcc-c++
```

2、PCRE pcre-devel 安装

PCRE(Perl Compatible Regular Expressions) 是一个Perl库，包括 perl 兼容的正则表达式库。nginx 的 http 模块使用 pcre 来解析正则表达式，所以需要在 linux 上安装 pcre 库，pcre-devel 是使用 pcre 开发的一个二次开发库。nginx也需要此库。命令：

```shell
yum install -y pcre pcre-devel
```

3、zlib 安装

zlib 库提供了很多种压缩和解压缩的方式， nginx 使用 zlib 对 http 包的内容进行 gzip ，所以需要在 Centos 上安装 zlib 库。

```shell
yum install -y zlib zlib-devel
```

4、OpenSSL 安装
OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及 SSL 协议，并提供丰富的应用程序供测试或其它目的使用。
nginx 不仅支持 http 协议，还支持 https（即在ssl协议上传输http），所以需要在 Centos 安装 OpenSSL 库。

```shell
yum install -y openssl openssl-devel
```

5、下载安装包

手动下载.tar.gz安装包，地址：https://nginx.org/en/download.html

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-22-11-36.png width='100%'/></div><br/>

下载完毕上传到服务器上 /root

6、解压

```shell
tar -zxvf nginx-1.18.0.tar.gz
cd nginx-1.18.0
```

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-22-12-19.png width='100%'/></div><br/>

7、配置

使用默认配置，在nginx根目录下执行

```shell
./configure
make
make install
```

查找安装路径：` whereis nginx `

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-22-17-15.png width='100%'/></div><br/>

### Nginx常用命令

```shell
cd /usr/local/nginx/sbin/
./nginx  启动
./nginx -s stop  停止
./nginx -s quit  安全退出
./nginx -s reload  重新加载配置文件（经常使用）
ps aux|grep nginx  查看nginx进程

```

启动成功访问 服务器ip:80

<div align='center'><img src=./images/Nginx-入门/Nginx-入门_2021-03-25-22-18-03.png width='100%'/></div><br/>

注意：如何连接不上，检查阿里云安全组是否开放端口，或者服务器防火墙是否开放端口！
相关命令：

```shell
# 开启
service firewalld start
# 重启
service firewalld restart
# 关闭
service firewalld stop
# 查看防火墙规则
firewall-cmd --list-all
# 查询端口是否开放
firewall-cmd --query-port=8080/tcp
# 开放80端口
firewall-cmd --permanent --add-port=80/tcp
# 移除端口
firewall-cmd --permanent --remove-port=8080/tcp
#重启防火墙(修改配置后要重启防火墙)
firewall-cmd --reload
# 参数解释
1、firwall-cmd：是Linux提供的操作firewall的一个工具；
2、--permanent：表示设置为持久；
3、--add-port：标识添加的端口；
```

### 演示 反向代理和负载均衡

```shell
upstream lb{
    server 127.0.0.1:8080 weight=1;
    server 127.0.0.1:8081 weight=1;
}
location / {
    proxy_pass http://lb;
}

```


