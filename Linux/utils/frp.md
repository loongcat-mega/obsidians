
## server

`frps.toml`
```toml
bindPort = 7000
```

### 启动服务

```bash
./frps -c ./frps.toml
or
# 启动frp
sudo systemctl start frps
# 停止frp
sudo systemctl stop frps
# 重启frp
sudo systemctl restart frps
# 查看frp状态
sudo systemctl status frps
# 设置为开机自启
sudo systemctl enable frps

```


## client


`frpc.toml`
```toml
serverAddr = "47.95.4.132"  
serverPort = 7000  
  
[[proxies]]  
name = "test-tcp"  
type = "tcp"  
localIP = "127.0.0.1"  
localPort = 22  
remotePort = 6000
```
将公网6000端口映射到私网22端口

### 启动服务

```bash
./frpc -c ./frpc.toml
```