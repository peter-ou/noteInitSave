# Docker的使用

## 一，如何在Linux服务器使用docker

1. 首先得是64位的系统，内核版本大于3.10。用root用户操作
内核版本查看指令

    `uname -r`

2. 卸载旧版本(如果以前安装过旧版本的话)

    ```linux
    yum remove docker \
        docker-common \
        docker-selinux \
        docker-engine
    ```

3. 安装依赖的软件包

    ```linux
    yum install -y yum-utils \
    device-mapper-persistent-data \
    lvm2
    ```

4. 设置yum源

    ```linux
    yum-config-manager \
        --add-repo \
        https://download.docker.com/linux/centos/docker-ce.repo
    ```

5. docker的安装

   + 查看docker版本列表

        `yum list docker-ce --showduplicates | sort -r`

   + 安装最新版docker

        `yum install -y docker-ce`

   + 安装指定版本docker（这里举例安装版本为   17.09.0.ce）

        `yum install -y docker-ce-17.09.0.ce`

6. 启动docker

    `systemctl start docker.service`

7. 查看docker版本并验证是否安装成功（有client和server即表示安装成功）

    `docker version`

8. docker的启动，停止，查看运行状态

   + 启动/重启docker

        `systemctl start/restart  docker`

   + 关闭docker

        `systemctl stop docker`

   + 查看docker的运行状态

       `systemctl status docker`

## 使用 docker 拉取nginx镜像和创建容器-nginx

1. 首先查找nginx镜像

    ```linux
     docker search nginx
    ```

2. 拉取官网docker-hup 中的nginx镜像

    > + 默认拉取的是最新的版本Using default tag: latest

    ```linux
    docker pull nginx
    ```

    > + 也可指定拉取版本 例如:拉取标签为1.15.5的nginx镜像

    ```linux
    docker pull nginx:1.15.5
    ```

3. 查看获取到的镜像文件

    ```linux
    [root@node1 ~]# docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    nginx               latest              7f70b30f2cc6        7 days ago          109MB
    ubuntu              14.04               a35e70164dfb        3 weeks ago         222MB
    ubuntu              17.10               1af812152d85        3 weeks ago         98.4MB
    ```

4. 启动nginx 容器

    ```linux
    [root@node1 ~]# docker run -d -p 80:80 nginx
    759d6d37858bcc70867321759df12d2881e6399204f2ba6a4b8b10aaea430da3
    ```

    + 报错
  
    ```linux
        [root@izwz9dc94s2y45exdo7dy0z ~]# docker run -d -p 80:80 nginx
    c91f3c93a34b58644954a4d2ff0bf743dde57f5d7e9bc62144fdfbcf4b51b468
    /usr/bin/docker-current: Error response from daemon: oci runtime error: container_linux.go:235: starting container process caused "process_linux.go:258: applying cgroup configuration for process caused \"Cannot set property TasksAccounting, or unknown property.\"".
    ```

5. 启动后nginx 验证

    直接浏览器总输入ip地址访问,出现welcome to nginx。到此通过docker 安装nginx完成。

6. 第4点中报错的原因（是Linux和docker版本兼容问题），报错解决办法：（这里以nginx为例，若是我们已经启动nginx容器）。

    + 查看当前正在运行的容器（当然也可以直接docker ps|grep nginx找到最精确的）
        `docker ps`

    + 查看正在运行中的所有容器,包括未运行的
        `docker ps -a`

    + 停止正在运行的容器(根据容器名称停止（当然也可以是id）)
        `docker stop name`

    + 删除所有nginx的容器，运行的和未运行的
        `docker rm 容器id`

    + 再查询nginx 的镜像

        `docker images`

    + 删除nginx镜像(#注意命令中相比容器多了一个i )

        ```linux
         docker rmi 镜像id
         docker rmi -f nginx        #强制删除nginx镜像
         docker rmi REPOSITORY:TAG  #当有2个一样的镜像时，这样删除
        ```

    + 容器停止，启动，重启

        ```linux
            docker ps                        #查看正在运行的容器
            docker ps -a                     #命令查看所有容器
            docker stop 容器名称/容器ID       #可以通过该命令直接停止某容器
            docker start 容器名称/容器ID      #可以通过该命令启动某容器：如果用run的话会新建一个新的容器
            docker restart 容器名称/容器ID    #可以通过该命令重启某容器
            其实，以nginx为例，流程是这样子的：拉取镜像后，用run启动容器，此时会新建一个容器，后面就不需要再新建了，直接用stop/start停止启动容器即可
        ```

    + 在删除了容器和镜像后，开始来解决Linux和docker版本兼容问题

        ```linux
            1、查看你当前的内核版本
            uname -r
            2、更新yum包
            sudo yum update
            3、卸载已安装的docker（如果安装过的话）
            yum remove docker  docker-common docker-selinux docker-engine
            4、安装需要的软件包
            sudo yum install -y yum-utils device-mapper-persistent-data lvm2
            5、设置yum源
            sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
            6、可以查看所有仓库中所有docker版本，并选择特定版本安装
            yum list docker-ce --showduplicates | sort -r
            7、重新安装docker
            sudo yum install docker-ce
            8、启动docker
            sudo systemctl start docker
            9、验证安装是否成功
            docker virsion  //查看docker的版本
            10、回到(使用 docker 拉取nginx镜像和创建容器-nginx目录下)
        ```
 