## 安装

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312141922226.png)

```
========================面板账户登录信息==========================

 外网面板地址: http://47.95.4.132:24971/0514b03b
 内网面板地址: http://172.19.16.65:24971/0514b03b
 username: yhcfqgfl
 password: 445e3fcc
 
=========================打开面板前请看===========================

 【云服务器】请在安全组放行 24971 端口
 因默认启用自签证书https加密访问，浏览器将提示不安全
 点击【高级】-【继续访问】或【接受风险并继续】访问
 教程：https://www.bt.cn/bbs/thread-117246-1-1.html
```

## 基本信息

![5b8388cb87394aac8e965dfeb1bcb60e.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312141939375.png)

```
1.系统操作
显示当前服务器系统类型、服务器运行时间、面板版本、面板升级更新入口、并提供宝塔账号绑定、微信小程序绑定、服务器重启、面板重启、修复面板等快捷功能。
2.服务器状态
显示当前服务器CPU、内存、硬盘的使用率、内存清理，所有状态均取自服务器真实数据。
内存的清理：点击内存图标中的小火箭图标，即可实现清理功能。
3.站点信息
显示当前面板管理的站点、FTP、数据库数量，仅提供数量显示，如需添加站点，请在网站选项中添加站点。
4.软件管理
首页软件快速方式，可以实现拖动图片，更换顺序、管理软件等功能。
5.网络流量
实时显示当前服务器网络流量的上传和下载速度，总上传流量，总下载流量。
```
## 创建网站站点

![49a5d86067ce4bec970b9cc41af89c09.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312141941031.png)
![97fec6024b884e8389c928e039338709.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312141941568.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312142016650.png)



启动

/etc/init.d/bt start

停止

/etc/init.d/bt stop


## 重启 nginx
```shell
nginx -s reload
```