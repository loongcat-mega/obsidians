![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240325101458.png)
```shell
删除所有信息之后update

sudo mv /var/lib/dpkg/info/ /var/lib/dpkg/info_old/
sudo mkdir /var/lib/dpkg/info/
sudo apt-get update

执行完以上代码后再用sudo apt-get install 安装
```