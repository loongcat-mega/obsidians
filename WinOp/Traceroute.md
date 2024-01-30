
可以定位从源主机到目标主机之间经过了哪些路由器，以及到大各个路由器的耗时

traceroute采用icmp报文（查询或差错报文）
运用icmp报文的TTL机制
```text
从源主机向目标主机发送IP数据报，并按顺序将TTL设置为从1开始递增的数字（假设为N），导致第N个节点（中间节点 or 目标主机）丢弃数据报并返回出错信息。源主机根据接收到的错误信息，确定到达目标主机路径上的所有节点的IP，以及对应的耗时。
```


```shell
# 假设想要知道，当我们访问 [http://www.iqiyi.com](http://www.iqiyi.com) 时，经过了多少中间节点，那么可以采用如下命令：
tracert www.iqiyi.com
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230426161048.png)

