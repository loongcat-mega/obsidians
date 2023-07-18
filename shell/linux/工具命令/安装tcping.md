
```
#更新系统软件源
sudo apt-get update -y

#安装包依赖
sudo apt-get install -y tcptraceroute bc

# 切换目录到/usr/bin
cd /usr/bin

# 下载TCP-PING可执行文件，并重命名为tcping
wget -O tcping https://soft.mengclaw.com/Bash/TCP-PING

# 赋予tcping执行权限
chmod +x tcping

```
