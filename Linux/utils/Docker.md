
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240101153107.png)

本地配置好环境后，将某个应用部署到其他终端或者服务器上，要面临重新部署环境的问题，为了模拟完全相同的开发环境，会想到使用虚拟机，但是虚拟机需要模拟整个硬件，运行操作系统，体积臃肿，内存占用高，程序性能收到影响，docker在概念上与虚拟机类似，但却轻量很多，不会模型底层底层硬件，只会为每个应用提供一个完全隔离的运行环境，可以在不同环境中配置不同工作软件，并且不同环境之间互不影响，这个环境在docker中被称为**容器container**


Dockerfile
像自动化脚本，创建镜像，类似于操作系统的安装引导程序

Image/镜像
虚拟机快照，里面包含了你要部署的应用程序以及它所关联的库

dashboard

Container/容器
通过镜像，我们可以创建多个不同的Container容器，这里的容器就像运行的虚拟机，每个容器独立运行，互不影响

 


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240101153934.png)








安装docker
`sudo apt install docker.io -y`

$ sudo apt-get update

$ sudo apt-get install  apt-transport-https ca-certificates  curl  gnupg lsb-release


配置docker用户权限
`sudo groupadd docker`
`sudo gpasswd -a $USER docker`
`newgrp docker`
`sudo chmod a+rw /var/run/docker.sock`