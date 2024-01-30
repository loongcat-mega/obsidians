强制杀死进程
taskkill /PID 101152 /F

## 网络命令

`ipconfig` :确定自己的网络ip地址

`ping` :判断网络是否通畅

`pathping` :确定每一个连接的关键点之间的延迟 
`pathping www.baidu.com /p 50 /h 3`

`msg` :将消息发送给用户
`msg /server:192.168.31.1 * "value" `
向ip地址为192.168.31.1的用户发送 value

msg /server:192.168.31.1 * "value"

`netsh` :网络有关配置
`netsh wlan show profile`
`netsh wlan show profile name key=clear`
将wlan名为name的密码以明文形式展现出来

`nslookup` :确定网络地址
`nslookup www.baidu.com`

`net share`：查看网络分享

`netstat` :网络状态
`netstat /a /n`:显示所有产生过的链接

## 文件和文件夹命令

`tree`:将当前盘的文件以树的形式打开

`dir`:展示当前文件夹下的所有文件

`mkdir Test`:创建名为Test的文件夹

`rd Test`:删除名为Test的文件夹

`echo`:编辑文件内容
`echo 1 > Test.txt`

`del Test.txt`:删除名为Test.txt的文件

`copy "a"+"b"  "c"`
a,b,c均为文件绝对路径，将a，b中的内容复制到c中

`type 1.txt` :查看1.txt的内容

`ren 1.txt 2.txt`:将1.txt 重命名为2.txt

## 系统信息命令

`systeminfo`:查看系统信息

`tasklist`:显示进程列表
`taskkill /IM aaa`:终止aaa进程

`shutdowm -s -t 600`:600s后自动关机
`shutdown -r -t 600`:600s后自动重启
`shutdowm -a`:取消定时关机

## 应用程序命令

`notepad`:记事本
`cleanmgr`:磁盘驱动清理器
`taskmgr`:任务管理器
`regedit`:注册表管理器
`perfmon`:性能监视器

查看内存最大支持容量 
`wmic memphysical get maxcapacity`


```shell
# 请勿用于商业用途，仅供个人测试学习之用，请遵守中国法律法规
# 查询本机外网IPv4地址
curl 4.ipw.cn

# 查询本机外网IPv6地址
curl 6.ipw.cn

# 测试网络是IPv4还是IPv6访问优先(访问IPv4/IPv6双栈站点，如果返回IPv6地址，则IPv6访问优先)
curl test.ipw.cn
```

```


DxDiag 查看系统信息
