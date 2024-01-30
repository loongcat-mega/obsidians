客户端上线以后就要服务器建立socket连接
如果有多个客户端就要建立多个持续监听的连接，资源问题？多线程解决？


## 多对一

### server
```python
from socket import *
import threading

def handle_client(conn,addr,client_id):
    print(f"accept new connection from{addr[0]}:{addr[1]},client_id={client_id} ")
    try:
        while True:
            sentence = conn.recv(1024).decode('utf-8')
            if sentence == "quit":
                print(f"client {client_id} leave")
                conn.close()
                return
            print(f"client {client_id}:{sentence}")
            response=f"I am server".encode('utf-8')
            conn.send(response)
    except Exception as e:
        print(f"Errot for client {client_id}:{e}")
    

serverPort=12000
serverSocket=socket(AF_INET,SOCK_STREAM)
serverSocket.bind(('',serverPort))
serverSocket.listen(5)

client_id_counter=100000

while True:
    conn,addr=serverSocket.accept()
    client_handler=threading.Thread(target=handle_client,args=(conn,addr,client_id_counter))
    client_handler.start()
    client_id_counter+=1
```


### client

```python
from socket import *

serverName = '47.95.4.132'
serverPort = 12000

clientSocket = socket(AF_INET, SOCK_STREAM)

clientSocket.connect((serverName, serverPort))

while True:
    message = input("Enter a message ('quit' to exit): ")
    clientSocket.send(message.encode('utf-8'))

    if message.lower() == 'quit':
        clientSocket.close()
        break
    response = clientSocket.recv(1024).decode('utf-8')
    print('Received from server:', response)

```


