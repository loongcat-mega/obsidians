下载SSH-server
`sudo apt-get install openssh-server`

开启ssh服务
`sudo systemctl start ssh`

安装net-tools工具
`sudo apt install net-tools`

启用root用户登录
`sudo chmod 777 /etc/ssh/sshd_config`
`vi /etc/ssh/sshd_config`
将`PermitRootLogin no`改为或添加`PermitRootLogin yes`

重启服务
`systemctl restart sshd`

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230511155216.png)

将上述文件内容清空

```bash
ssh -o Port=6000 "3470901328@qq.com"@172.16.43.125
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312132320424.png)


```bash
ssh -p 6000 -o "ProxyJump=47.95.4.132:6000" dlut2102@172.16.43.125

ssh -p 6000 dlut2102@47.95.4.132


msg /server:47.95.4.132 * "value"

echo "aaa" |nc 47.95.4.132:8080
```
