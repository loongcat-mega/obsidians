

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240117183100.png)

套接字是一种进程间通信的方法，他不局限于同一台计算机的资源，例如文件系统空间，共享内存或者消息队列
同一台机器的进程间也可以用套接字通信

socket用来描述IP地址和端口，是通信链的句柄，应用程序可以通过Socket向网络发送请求或应答，Socket是支持TCP、IP协议的网络通信的基本操作单元，是对网络通信过程中端点的抽象表示，包含了进行网络通信所必须的第五种信息：连接所使用的协议；本地主机的IP；本地远程的协议端口；远地主机的IP及端口

## linux套接字

套接字的特性由三个属性决定：
- 域 domain
- 类型 type
- 协议 protocol

域 ：指定套接字通信中使用的网络介质 包括地址格式
- AF_INET  互联网络，套接字地址由IP地址+端口号决定
- AF_UNIX  基于本地机器，地位为绝对路径的文件名

类型：不同的通信方式
- 流套接字 SOCK_STREAM,基于TCP/IP实现
- 数据包套接字 SOCK_DGRAM，基于UDP/IP实现

协议:我们重点讨论UNIX网络套接字和文件系统套接字，不需要选择特定协议，只要默认值（0）即可

```c
//socket系统调用创建一个套接字，并返回一个描述符，该描述符可以用来访问这个套接字。创建的套接字是一条通信链路的一个端点
int socket( int domain, int type, int protocol);
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231224210456.png)







## python套接字





![37360672.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312151605545.png)




![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312151620755.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312151621876.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312151625271.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312151626140.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312151626576.png)
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312151626680.png)
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312151626976.png)
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312151627418.png)
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312151628109.png)





### Client

```python
# -*- coding: utf-8 -*-
from socket import *
serverName = '47.95.4.132' # 指定服务器地址
serverPort = 12000
clientSocket = socket(AF_INET, SOCK_STREAM)
 # 建立TCP套接字，使用IPv4协议，第二参数为类型声明，和上面UDP套接字不同；
 # 创建客户套接字时没有指定 端口号，操作系统进行该操作
clientSocket.connect((serverName,serverPort)) # 向服务器发起连接，先进行三次握手，然后建立TCP连接；
# 注意和UDP方式对比：UDP直接分组然后发送（sendto）
while True:
    sentence = input('Input  sentence:').encode('utf-8') # 用户输入信息，并编码以便发送
    clientSocket.send(sentence) # 将信息发送到服务器
    print(f"{sentence.decode('utf-8')} 发送成功")
    print("等待接收")
    recvsentence = clientSocket.recv(1024).decode('utf-8') # 从服务器接收信息
    print(recvsentence) # 显示信息
clientSocket.close() # 关闭套接字

```

### Server

```python
from socket import *

serverPort = 12000
serverSocket = socket(AF_INET, SOCK_STREAM)
serverSocket.bind(('', serverPort))
serverSocket.listen(1)
print("The server is ready to receive")

while True:
    connectionSocket, addr = serverSocket.accept()
    print('Accept new connection from %s:%s...' % addr)
    sentence = connectionSocket.recv(1024).decode('utf-8') # 获取客户发送的字符串
    print(sentence)
    sentence = input('Input sentence:').encode('utf-8') # 用户输入信息，并编码以便发送
    connectionSocket.send(sentence)
    print(f"{sentence.decode('utf-8')} 发送成功")
    print("等待接收")
    connectionSocket.close()
```

