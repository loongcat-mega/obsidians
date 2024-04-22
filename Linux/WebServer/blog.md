
## 安装宝塔面板

```shell
wget -O install.sh https://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh ed8484bec
```

更改宝塔面板端口、名字、密码

## 进入宝塔面板安装LNMP

Linux+Nginx+MySQL+PHP
- Nginx是一个高性能的HTTP和反向代理服务器，也是一个IMAP/POP3/SMTP代理服务器  
- Mysql是一个小型关系型数据库管理系统  
- PHP是一种在服务器端执行的嵌入HTML文档的脚本语言


LNMP代表的就是：Linux系统下Nginx+MySQL+PHP这种网站服务器架构

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240405090902.png)

## 安装wordpress

WordPress是使用PHP语言（这也是我们上面为什么要安装 PHP 的原因）开发的博客平台，也就是一个博客框架
```shell
wget https://cn.wordpress.org/latest-zh_CN.zip && unzip latest-zh_CN.zip -d /home/wwwroot
```
修改nginx配置
```shell
vim /www/server/nginx/conf/server/nginx/conf/nginx.conf
```
将网站根目录设置为wordpress
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240405091332.png)
修改wordpress目录权限
```shell
 cd /home/wwwroot && chown -R www wordpress/ && chgrp -R www wordpress/
```
验证nginx配置
```shell
nginx -t


nginx -s reload

```

## wordpress预配置

用浏览器打开http://ip/wp-admin/setup-config.php
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240405091606.png)
现在开始
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240405091624.png)
配置用户信息
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240405091650.png)

## 管理wordpress

### 主题：argon

### 插件

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240405092242.png)

### 上传文件

出于安全考虑，WordPress 默认限制了能上传的文件类型。默认允许上传的文件类型有：

图片：.jpg .png .gif .jpeg .ico  
文件：.pdf .doc .ppt .odt .xls .psd  
音频：.mp3 .m4a .ogg .wav  
视频：.mp4 .mov .avi .mpg .ogv .3gp .3g2

这些文件比较安全，不会影响 WordPress 正常运行。

突破限制：Extra File Types 插件

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240405093422.png)
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240405093434.png)
找GPT问对应文件的mime类型

### 下载文件

```html
<a href="http://182.92.170.252:888/wp-content/uploads/ckpts/EP4/setup.sh" download>EPOCH4</a>
```
