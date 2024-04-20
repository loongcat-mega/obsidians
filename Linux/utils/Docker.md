# Docker基本


## 安装docker

```shell
 curl -fsSL https://test.docker.com -o test-docker.sh
 sudo sh test-docker.sh
```

```shell
sudo apt-get update
sudo apt-get install  apt-transport-https ca-certificates  curl  gnupg lsb-release
```

配置docker用户权限
```shell
sudo groupadd docker
sudo gpasswd -a $USER docker
newgrp docker
sudo chmod a+rw /var/run/docker.sock
```
`sudo groupadd docker`
`sudo gpasswd -a $USER docker`
`newgrp docker`
`sudo chmod a+rw /var/run/docker.sock`


开启docker服务器
```shell
sudo service docker start
```

## Docker是什么

Docker是一个用于 构建 运行 传送 应用程序的平台

将应用程序和他的运行时环境、配置文件等打包在一起
本地配置好环境后，将某个应用部署到其他终端或者服务器上，要面临重新部署环境的问题，为了模拟完全相同的开发环境，会想到使用虚拟机，但是虚拟机需要模拟整个硬件，运行操作系统，体积臃肿，内存占用高，程序性能收到影响，docker在概念上与虚拟机类似，但却轻量很多，不会模型底层底层硬件，只会为每个应用提供一个完全隔离的运行环境，可以在不同环境中配置不同工作软件，并且不同环境之间互不影响，这个环境在docker中被称为**容器container**

为了解决花大量时间配置环境的问题
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240401223726.png)

## Docker与虚拟机的区别
容器虚拟化操作系统
虚拟机虚拟化硬件
Docker只是容器的一种实现形式
容器和虚拟机都是虚拟化技术
需要的是完整的操作系统还是应用接口？
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240401224319.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240401224332.png)

## 基本概念
Dockre是使用cs架构的

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240401225409.png)

### 镜像
只读的模版，用来创建容器实例

Alpine 显然不适合作为Python 应用的基础映像。尽管它提供了惊人存储的空间上效率，但它对于Python包支持的不足的缺陷是难以弥补的。也许Alpine更适合于一些对于映像尺寸敏感的场合，还可以考虑将它用于你的Go 应用

### 容器

是Docker的运行实例，提供独立的可移植 运行环境

镜像与容器就像类和实例？deamon

### 仓库

存储镜像s的地方，DockerHub
便于实现镜像的共享和复用

### 容器化
将应用程序打包成容器，然后在容器中运行应用程序的过程
- 创建一个Dockerfile（配置文件）
- 使用Dockerfile构建镜像
- 使用镜像创建和运行容器

像自动化脚本，创建镜像，类似于操作系统的安装引导程序


## 容器

### docker version
查看docker版本，client和server，如果只能看到client，说明docker没有启动

### Dockerfile

```Dockerfile
FROM node:14-alpine
COPY index.js /index.js
CMD node /index.js
```



### 运行容器

```shell
docker run docker-name
```
```shell
runoob@runoob:~$ docker run ubuntu:15.10 /bin/echo "Hello world"
Hello world

docker run image cmd
```

#### 交互式运行

```shell
runoob@runoob:~$ docker run -i -t ubuntu:15.10 /bin/bash
root@0123ce188bd8:/#
```
-   **-t:** 在新容器内指定一个伪终端或终端。
    
-   **-i:** 允许你对容器内的标准输入 (STDIN) 进行交互。

退出：`exit`
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402100232.png)


#### 后台启动
`-d`
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402100431.png)
输出的字符串为docker的id

### 查看docker运行状况
```shell
docker ps
docker pa -a
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402100431.png)
**CONTAINER ID:** 容器 ID。
**IMAGE:** 使用的镜像。
**COMMAND:** 启动容器时运行的命令。
**CREATED:** 容器的创建时间。
**STATUS:** 容器状态。
状态有7种：
-   created（已创建）
-   restarting（重启中）
-   running 或 Up（运行中）
-   removing（迁移中）
-   paused（暂停）
-   exited（停止）
-   dead（死亡）

**PORTS:** 容器的端口信息和使用的连接类型（tcp\udp）。

**NAMES:** 自动分配的容器名称。


### 查看docker标准输出

```shell
docker logs did/dname
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402100722.png)


### 停止容器

```shell
docker stop did/dname
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402100834.png)


### 启动一个停止的容器

```shell
docker start did/dname
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402101145.png)


### 重启容器

```shell
docker restart did/dname
```

### 进入容器

使用-d参数时，容器会进去后台运行状态
```shell
docker exec did/dname
docker attach did/dname
```
使用exec 退出不会导致容器停止
使用attach会导致容器停止

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402102104.png)

### 指定映射端口

`-P`:端口随机映射
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402102825.png)

docker开放了tcp 5000 端口，映射到主机32768上


```shell
docker run -d -p 5001:5000 training/webapp python app.py
```
将容器内5000端口，映射到本地5001端口

使用`docker port did/dname`查看指定容器端口映射情况

### 查看容器内进程
```shell
docker top did/dname
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402103029.png)

### 文件传输

```shell
docker cp [source] [dest]

docker cp ./run.py musing_lalande:/longcat
docker cp ./run.py musing_lalande:/longcat/
```
### 数据卷

数据卷是一个可供容器使用的特殊目录，它使得容器可以持久化存储数据。

#### 创建数据卷

```shell
docker volume create vname
```
创建名为vname的数据卷

#### 查看已由的数据卷

```shell
docker volume ls
```
#### 挂载数据卷

```shell
docker run -v vname:/longcat did/dname
```
将创建的数据卷挂载到容器的logncat目录
这将创建一个新容器，并将 `vname` 数据卷挂载到容器的 `/longcat` 目录下。现在，容器内的 `/longcat` 目录将使用数据卷 `vname` 的数据
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402160750.png)



#### 删除数据卷

```shell
docker volume rm vname
```



### 导入和导出

导出本地容器快照到name.tar文件中
```shell
docker export did/dname > name.tar
```
导入容器到镜像快照
```shell
docker import name.tar test/ubuntu:v1
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402101907.png)


### 删除容器

```shell
docker rm -f  did/dname
```
下面的命令可以清理掉所有处于终止状态的容器。

```shell
docker container prune
```

## 镜像

### 构建镜像
```shell
docker build -t iname Dockerfile_path
```
按照Dockerfile文件构建的镜像名为iname，Dockerfile文件路径为`.`


### 更新镜像

```shell
docker commit -m "message" did/dname iname
```
将did/dname容器 更新到某个镜像（如果没有回自动创建）
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402103800.png)

### 查看镜像
```shell
docker image ls
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402095624.png)
```txt

REPOSITORY：表示镜像的仓库源

TAG：镜像的标签

IMAGE ID：镜像ID

CREATED：镜像创建时间

SIZE：镜像大小
```
同一仓库源可以有多个 TAG，代表这个仓库源的不同个版本

### 查找镜像

```shell
docekr search iname
```
依据镜像名称去hub查找镜像
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402103319.png)

### 为镜像添加标签
```shell
docker tag iid/iname new_tag
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402104252.png)

为同一个镜像添加多个标签，镜像id相同
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402104412.png)

### 拉取镜像

```shell
docker pull docker-url
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240401231704.png)


### 删除镜像

```shell
docker rmi iname
```
删除指定名字的镜像
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402103439.png)

## Dockerfile
Dockerfile 是一个用来构建镜像的文本文件，文本内容包含了一条条构建镜像所需的指令和说明。

```Dockerfile
FROM python:3.8
COPY run.py /run.py
RUN pip install fastapi  \
    && pip install "uvicorn[standard]" 

```
定制的镜像都是基于FROM的镜像
Dockerfile 的指令每执行一次都会在 docker 上新建一层。所以过多无意义的层，会造成镜像膨胀过大
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402110703.png)

### FROM
基础镜像

### RUN
运行指定命令
### COPY

复制指令，从上下文目录中复制文件或者目录到容器里指定路径
```shell
COPY [host_sour] [docker_dest]
```
如果路径不存在，会自动创建

### CMD

用于运行程序，与RUN不同的是，CMD是在docker run时运行，RUN是在docker build时运行
只有最后一个CMD命令生效





# MYSQL

```bash
docker pull ubuntu/myql
docker run  -p 3308:3306 -e MYSQL_ROOT_PASSWORD=123456 ubuntu/mysql

docker run  -v /usr/local/mysql/logs:/logs -v /usr/local/mysql/data/mysql:/var/lib/mysql  -p 3308:3306 --name mysql -e MYSQL_ROOT_PASSWORD=123456 ubuntu/mysql &


```

#  NVIDIA-Docker
看不懂一点，哪里有这些匹配的镜像？
cuda版本？cudnn版本?ubuntu版本？Mindspore版本？
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402170255.png)

```shell
docker pull nvidia/cuda:11.6.2-runtime-ubuntu18.04

docker pull swr.cn-south-1.myhuaweicloud.com/mindspore/mindspore-gpu-cuda11.6:2.2.12
```


[Docker容器如何优雅使用NVIDIA GPU](https://cloud.tencent.com/developer/article/1924792#Popover19-toggle:~:text=Docker%E5%AE%B9%E5%99%A8%E5%A6%82%E4%BD%95%E4%BC%98%E9%9B%85%E4%BD%BF%E7%94%A8NVIDIA%20GPU)
[Nvidia-Docker 教程](https://longxinglx.github.io/posts/Nvidia-Docker/#Popover19-toggle:~:text=Nvidia-Docker%20%E6%95%99%E7%A8%8B)

[安装 NVIDIA 容器工具包](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#Popover19-toggle:~:text=%E5%AE%89%E8%A3%85%20NVIDIA%20%E5%AE%B9%E5%99%A8%E5%B7%A5%E5%85%B7%E5%8C%85%EF%83%81)

https://github.com/NVIDIA/nvidia-docker#quick-start

# Mindspore-Docker

[获取安装命令](https://www.mindspore.cn/install/#Popover19-toggle:~:text=%E8%8E%B7%E5%8F%96%E5%AE%89%E8%A3%85%E5%91%BD%E4%BB%A4)



### 拉取镜像
```shell
docker pull swr.cn-south-1.myhuaweicloud.com/mindspore/mindspore-gpu-cuda11.6:2.2.12
```
### nvidia-container-toolkit安装
```shell

DISTRIBUTION=$(. /etc/os-release; echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$DISTRIBUTION/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit nvidia-docker2
sudo systemctl restart docker

```

daemon.json是Docker的配置文件，编辑文件daemon.json配置容器运行时，让Docker可以使用nvidia-container-runtime:

```shell

$ vim /etc/docker/daemon.json
{
    "runtimes": {
        "nvidia": {
            "path": "nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}

```

再次重启Docker:

```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### 运行

```shell
docker run -it -v /dev/shm:/dev/shm --runtime=nvidia swr.cn-south-1.myhuaweicloud.com/mindspore/mindspore-gpu-{cuda_version}:{tag} /bin/bash

 docker run -it -v /mnt/f/MindSporeMCAN/:/MindSporeMCAN --runtime=nvidia swr.cn-south-1.myhuaweicloud.com/mindspore/mindspore-gpu-cuda11.6:2.2.12 /bin/bash 
```
### 验证是否成功安装


```
python -c "import mindspore;mindspore.set_context(device_target='GPU');mindspore.run_check()"
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240402183550.png)


# BASH



```shell
## 安装docker
curl -fsSL https://test.docker.com -o test-docker.sh
sudo sh test-docker.sh

## nvidia-container-toolkit安装
# Acquire version of operating system version
DISTRIBUTION=$(. /etc/os-release; echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$DISTRIBUTION/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit nvidia-docker2
sudo systemctl restart docker

config='{
    "runtimes": {
        "nvidia": {
            "path": "nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}'

echo "$config" | sudo tee /etc/docker/daemon.json >/dev/null


## 再次重启docker
sudo systemctl daemon-reload
sudo systemctl restart docker

## 下载官方docker
##docker pull swr.cn-south-1.myhuaweicloud.com/mindspore/mindspore-gpu-cuda11.6:2.1.1
```
