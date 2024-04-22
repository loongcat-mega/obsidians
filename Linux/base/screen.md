
## base


-r <作业名称> 　恢复离线的screen作业。  
-R 　先试图恢复离线的作业。若找不到离线的作业，即建立新的screen作业。  
-s 　指定建立新视窗时，所要执行的shell。  
-S <作业名称> 　指定screen作业的名称。  。  
-x 　恢复之前离线的screen作业。  
-ls或--list 　显示目前所有的screen作业。  
-wipe 　检查目前所有的screen作业，并删除已经无法使用的screen





## 查看已经打开的窗口
```text
screen -ls
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240329183729.png)
97960为运行的窗口的pid
log为窗口的名称
screen有两种状态，`Attached`状态，其实代表此虚拟终端，用户正在使用，这个时候，是无法进入的。
但是，有时候，我们创建虚拟终端，并没有使用`Ctril+a`再按`d`退出并挂起虚拟终端，反而因为长时间没操作，或者本地网络掉包等问题，非正常退出虚拟终端，导致出现SSH连接服务器，并没有在虚拟终端内，却出现`Attached`状态

这个时候，不用慌。只需要：

```text
screen -d tool
```

screen -d操作

之后，即可使用`screen -r tool`或`screen -R tool`进入。
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240329231148.png)
死了？？？？

## 创建新窗口
```text
创建一个叫Hello的虚拟终端
screen -S Hello

```
-   使用`-R`创建，如果之前有创建唯一一个同名的screen，则直接进入之前创建的screen
-   使用`-S`创建和直接输入`screen`创建的虚拟终端，不会检录之前创建的screen（**也就是会创建同名的screen**)

## 保持窗口运行并退出

按Ctril+(a+d)，即可保持这个screen到后台并回到主终端
screen -d -r yourname -> 结束当前session并回到yourname这个session

## 回到指定窗口

```text
使用screen -r命令
恢复离线的screen作业
screen -r [pid/name]
```
screen -d yourname -> 远程detach某个session
## 退出窗口


```txt
ctrl+A+k
或

使用-R/-r/-S均可
screen -R [pid/Name] -X quit

```

## 发送命令到窗口

```shell
screen -S sandy -X screen ping www.baidu.com
```


## 共享窗口

```shell
screen -x
```
