
该部分实现了pegasus与taurus板子间通过uart串口传输数据，并将传输的数据发送到lwip服务器端，最后将lwip服务器端的数据发送到本地数据库的功能

# pagasus与taurus互联


# pegasus基于LWIP协议的TCP/IP通信


# 核心代码

## 初始化uart 与 lwip设置 并将收到的数据发送到lwip server端


```c
static hi_void *UartDemoTask(char *param)

{

    hi_u8 uartBuff[UART_BUFF_SIZE] = {0};

    hi_unref_param(param);

    printf("Initialize uart demo successfully, please enter some datas via DEMO_UART_NUM port...\n");

    Uart1GpioCOnfig();

    //

    int netId = ConnectToHotspot();

  

    int timeout = 3; /* timeout 10ms */

    while (timeout--)

    {

        printf("After %d seconds, I will start lwip test!\r\n", timeout);

        osDelay(100); /* 延时100ms */

    }

  

    printf("TcpClientTest start\r\n");

    printf("I will connect to %s\r\n", PARAM_SERVER_ADDR);

    int sockfd = socket(AF_INET, SOCK_STREAM, 0); // TCP socket

    struct sockaddr_in serverAddr = {0};

    serverAddr.sin_family = AF_INET;                // AF_INET表示IPv4协议

    serverAddr.sin_port = htons(PARAM_SERVER_PORT); // 端口号，从主机字节序转为网络字节序

    if (inet_pton(AF_INET, PARAM_SERVER_ADDR, &serverAddr.sin_addr) <= 0)

    { // 将主机IP地址从“点分十进制”字符串 转化为 标准格式（32位整数）

        printf("inet_pton failed!\r\n");

        lwip_close(sockfd);

    }

    // 尝试和目标主机建立连接，连接成功会返回0 ，失败返回 -1

    if (connect(sockfd, (struct sockaddr *)&serverAddr, sizeof(serverAddr)) < 0)

    {

        printf("connect failed!\r\n");

        lwip_close(sockfd);

    }

    printf("connect to server %s success!\r\n", PARAM_SERVER_ADDR);

  

    for (;;)

    {

        uartDefConfig.g_uartLen = IoTUartRead(DEMO_UART_NUM, uartBuff, UART_BUFF_SIZE);

        if ((uartDefConfig.g_uartLen > 0) && (uartBuff[0] == 0xaa) && (uartBuff[1] == 0x55))

        {

            if (GetUartConfig(UART_RECEIVE_FLAG) == HI_FALSE)

            {

                (void)memcpy_s(uartReadBuff, uartDefConfig.g_uartLen,

                               uartBuff, uartDefConfig.g_uartLen);

                (void)SetUartRecvFlag(UART_RECV_TRUE);

                TaskMsleep(20); /* 20:sleep 20ms */

                uartReadBuff[uartDefConfig.g_uartLen] = '\n';

                ssize_t retval = send(sockfd, uartReadBuff, UART_BUFF_SIZE, 0);

                if (retval < 0)

                {

                    printf("send uartReadBuff failed!\r\n");

                    lwip_close(sockfd);

                }

                printf("send uartReadBuff{%s} ,%ld to server done!\r\n", uartReadBuff, retval);

                usleep(U_SLEEP_TIME * 5);

            }

        }

    }

    return HI_NULL;

}
```

## 将lwip server端接受到的数据转换为十六进制字符串
```java
String data=br.readLine();  
            StringBuilder sb = new StringBuilder();  
            while(data!=null)  
            {  
                for (int i = 0; i < data.length(); i++) {  
                    char c = data.charAt(i);  
                   // System.out.println("c=="+c);  
                    String binary = String.format("%16s", Integer.toBinaryString(c)).replace(' ', '0');  
                    //System.out.println("binary=="+binary);  
                    String l8_binary = binary.substring(8); // 提取后8位  
  
                    String hex = binaryToHex(l8_binary);  
                    bw.write(hex);  
                    sb.append(hex);  
                }  
                data=br.readLine();  
            }  
            String result = sb.toString(); // 格式化为字符串
            
```

## 获取发送的class_id xmin xmax等属性

```java
List<String> pairs=findMatchingPairs(result);
```
## 将得到的字符串数据进行十进制处理

```java
List<Integer> dec=new ArrayList<>();  
  
String substring32 = s.substring(8, 40);  
//System.out.println("substring32=="+substring32);  
String lastTwo = s.substring(s.length() - 2);  
// System.out.println("lastTwo=="+lastTwo);  
for (int i = 0; i < 32; i += 8) {  
String binaryString = substring32.substring(i, i + 8);  
//System.out.println("binaryString=="+binaryString);  
int decimal =0;  
for(int j=0;j<8;j+=2)  
{  
    String decimalString = binaryString.substring(j, j + 2);  
    decimal = decimal*10 +Integer.parseInt(decimalString); 
    dec.add(decimal);  
}

```
## 将class_id,xmin,ymin,xmax,ymax发送到本地数据库进行存储


