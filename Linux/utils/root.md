
##### `sudo`	管理员权限

##### `sudo useradd -m test1`	添加名为test1的用户，其中`/m`参数表示在`/home`下添加用户目录

##### `sudo passwd test1`	 修改test1用户的密码

##### `sudo userdel test1`	删除test1用户

​	以上三个用户目录前不需要加`/`

##### `sudo rm -rf /home/test1`	删除home目录下test1的目录

##### `sudo passwd root` 	超级用户，首次使用需设置密码

##### `su root`	切换到超级用户

##### `exit`	退出超级用户，返回

​		超级用户仅对当前对话框（终端）有效。

### 启用root

`sudo passwd root`


### 以root用户登陆

##### `su root` 	切换到root用户

##### `gedit /etc/pam.d/gdm-autologin`	在第三行开头加#（注释掉）

##### `gedit /etc/pam.d/gdm-password`	第三行开头加#

##### 关闭虚拟机重启->用户未列出->用户名root->密码->进入root桌面