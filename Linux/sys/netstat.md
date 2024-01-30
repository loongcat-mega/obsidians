
1.  **显示所有连接和监听端口：**
    
   
    `netstat`
    
2.  **显示所有连接和监听端口（数字格式）：**
    
    
    `netstat -n`
    

### 显示监听端口：

3.  **显示所有正在监听的 TCP 端口：**
    
    
    `netstat -tul`
    
4.  **显示所有正在监听的 UDP 端口：**
    
    
    `netstat -u`
    
5.  **显示所有正在监听的 TCP 和 UDP 端口：**
    
    
    `netstat -l`
    

### 显示网络连接：

6.  **显示所有 TCP 连接：**
    
    
    `netstat -t`
    
7.  **显示所有 UDP 连接：**
    
    
    `netstat -u`
    
8.  **显示所有 TCP 和 UDP 连接：**
    
    
    `netstat -a`
    

### 显示详细信息：

9.  **显示 PID 和进程名称（需要管理员权限）：**
    
    
    `sudo netstat -tulpn`
    

### 过滤结果：

1.  **查看特定端口是否在监听：**
    
    
    `netstat -tuln | grep <端口号>`


1. 
2.  netstat -an | findstr ":22"
3. 
显示 IPv4 连接：
    
    
    `netstat -4`
    
5.  **显示 IPv6 连接：**
    
    
    `netstat -6`
    

### 更多选项：

13.  **显示统计信息：**
    
    
    `netstat -s`
    
14.  **显示核心信息（需要管理员权限）：**
    
    
    `sudo netstat -c`
    

这只是 `netstat` 命令的一些常见用法。你可以在终端中输入 `man netstat` 获取完整的 `netstat` 命令手册，以查看更多选项和详细信息。