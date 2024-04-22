![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240329225154.png)
默认一分钟后关机
- k 不执行任何关机操作，只发出警告信息给所有用户

- r 重新启动计算机

- h 关机并彻底断电

- f 快速关机且重启动时跳过fsck

- n 快速关机不经过init程序

- c 取消之前的定时关机

```bash
五分钟后关机
shutdown -h +5

五分钟后重启
shutdown -r +5

20:13重启
shutdown -h 20:13

100s后重启
shutdown -r -t 100
```