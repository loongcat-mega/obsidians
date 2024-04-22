
## init

历史上，Linux 的启动一直采用init 进程。

在类Unix 的计算机操作系统中，Init（初始化的简称）是在启动计算机系统期间启动的第一个进程。

Init 是一个守护进程，它将持续运行，直到系统关闭。它是所有其他进程的直接或间接的父进程

## service

通过查看man 手册页可以得知，service是一个运行System V init的脚本命令。

那么什么是 System V init 呢？  
也就是/etc/init.d 目录下的参数。

所以分析可知service 是去/etc/init.d目录下执行相关程序，服务配置文件的存放目录就是/etc/init.d.

使用 service 启动一个服务

## systemd

Systemd 就是为了解决这些问题而诞生的。它包括 System and Service Manager，为系统的启动和管理提供一套完整的解决方案。  
Systemd 是Linux 系统中最新的初始化系统（init），它主要的设计目的是克服 System V init 固有的缺点，提高系统的启动速度。

根据 Linux 惯例，字母d是守护进程（daemon）的缩写。 Systemd 这个名字的含义，就是它要守护整个系统。

使用了 Systemd，就不需要再用init 了。Systemd 取代了initd（Initd 的PID 是0） ，成为系统的第一个进程（Systemd 的PID 等于 1），其他进程都是它的子进程。

_Systemd 的优点是功能强大，使用方便，缺点是体系庞大，非常复杂_


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240406143926.png)
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240406144011.png)
