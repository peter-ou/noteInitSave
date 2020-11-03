# 04window环境nginx部署静态资源一

- [04window环境nginx部署静态资源一](#04window环境nginx部署静态资源一)
  - [Windows下Nginx的启动、停止等命令<a id="id1"></a>](#windows下nginx的启动停止等命令)
  - [利用nginx搭建静态资源服务器](#利用nginx搭建静态资源服务器)

## Windows下Nginx的启动、停止等命令<a id="id1"></a>

> 在Windows下使用Nginx，我们需要掌握一些基本的操作命令，比如：启动、停止Nginx服务，重新载入Nginx等，下面我就进行一些简单的介绍。
> 假设你的Nginx安装在 C:\server\nginx-1.0.2目录下

1. cmd命令进入安装文件

    ```dos
    1. windows+R => cmd  进入 命令提示符（最好以管理员的身份运行）

    2. C:   #切换到磁盘C中

    3. cd server\nginx-1.0.2  #进入到nginx的安装目录
    ```

2. 启动：

    `C:\server\nginx-1.0.2>start nginx`
    或
    `C:\server\nginx-1.0.2>nginx.exe`

     注：建议使用第一种，第二种会使你的cmd窗口一直处于执行中，不能进行其他命令操作。

3. 停止：

    `C:\server\nginx-1.0.2>nginx.exe -s stop`
    或
    `C:\server\nginx-1.0.2>nginx.exe -s quit`

     注：stop是快速停止nginx，可能并不保存相关信息；quit是完整有序的停止nginx，并保存相关信息。

4. 重新载入Nginx：

    `C:\server\nginx-1.0.2>nginx.exe -s reload`

    当配置信息修改，需要重新载入这些配置时使用此命令。

5. 查看Nginx版本：

    `C:\server\nginx-1.0.2>nginx -v`

6. windows上如何查看nginx是否启动成功？

    查看方法一：

    在任务栏空白处右击，弹出菜单中选择"任务管理器" => 弹出的对话框中 选中 “详细信息”。如果nginx服务启动了话，在任务管理器中的详细信息中可以看到它的进程，否则则表示未正常启动。

    查看方法二：

    到nginx的安装目录中到logs目录下查看是否存在nginx.pid文件。nginx启动后会在logs目录下生成nginx.pid文件，停止后会自动删除。所以也可以通过查看该目录下是否生成此文件，来判断nginx服务是否启动。有nginx.pid文件，启动成功，反之则未运行。

## 利用nginx搭建静态资源服务器

1. 我电脑上的 E:\staticpath\cdgbimg 路径下面有很多图片，我想通过nginx搭建静态资源服务器，通过在浏览器地址栏输入ip+port的方式完成目录的映射

2. 找到nginx安装目录，打开/conf/nginx.conf配置文件，添加一个**虚拟主机**（也就是添加一个location配置）

    ```json
        server {
                listen       80;  #监听端口
                server_name  localhost;  #访问域名

                #charset koi8-r;

                #access_log  logs/host.access.log  main;

                location / {
                    root   html;
                    index  index.html index.htm;
                }

                #重点是添加location
                #这个是用alias自定义配置的静态资源(图片)路径
                location /backgroud/ {
                    #经过测试：文件夹命名时不能有空格，下划线，中文等。建议用纯小写字母命名文件夹
                    alias  E:/staticpath/cdgbimg/;
                    autoindex on;
                }

                #这个是用root配置的静态资源(图片)路径
                location /cdgbimg/ {
                    root  E:/staticpath/;
                    autoindex on;
                }
                #error_page  404              /404.html;

    ```

    添加监听端口、访问域名，重点是添加location，映射-URL：/backgroud/

    开启浏览目录权限：autoinedx on，默认是off；

    注意：如果当前server模块中已有一个location且URL为“/”,那么新建的location的url应为匹配路径，不得再为“/”,此时，映射路径可不是随便写的，首先是你的root目录下面必须有这个URL目录，否则会报404（这一规则当时可是害惨我了）；

    alias和root 的区别
    <div align='center'><img src=./images/04window环境nginx部署静态资源一/04window环境nginx部署静态资源一_2020-05-24-23-46-28.png width='90%'/></div><br/>

3. 然后保存，启动nginx [请参考上面的Windows下Nginx的启动、停止等命令](#id1)

4. 浏览器地址栏输入：http://localhost:80/backgroud/

    > 显示如下图，则配置成功！

<div align='center'><img src=./images/04window环境nginx部署静态资源一/04window环境nginx部署静态资源一_2020-05-24-23-13-30.png width='80%'/></div><br/>
