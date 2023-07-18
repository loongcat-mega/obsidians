[lwIP TCP/IP 协议栈笔记之一：概述和目录结构详解](https://blog.csdn.net/XieWinter/article/details/97629336#Popover19-toggle:~:text=%0D%0AlwIP%20TCP%2FIP%20%E5%8D%8F%E8%AE%AE%E6%A0%88%E7%AC%94%E8%AE%B0%E4%B9%8B%E4%B8%80%EF%BC%9A%E6%A6%82%E8%BF%B0%E5%92%8C%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84%E8%AF%A6%E8%A7%A3%0D%0A)

```
lwIP（A Lightweight TCP/IP stack）是瑞典计算机科学院(SICS)的Adam Dunkels 开发的一个小型开源的TCP/IP协议栈。LwIP是Light Weight (轻型)IP协议，有无操作系统的支持都可以运行。LwIP实现的重点是在保持TCP协议主要功能的基础上减少对RAM 的占用，它只需十几KB的RAM和40K左右的ROM就可以运行。这样就可以让lwIP适用于资源有限的小型平台例如嵌入式系统。
```

lwIP 主要特性：

      *IP（Internet协议，IPv4和IPv6），包括通过多个网络接口的数据包转发
      ICMP 用于网络维护和调试（Internet控制消息协议）
      IGMP 用于多播流量管理（因特网组管理协议）
      *MLD（IPv6的多播侦听器发现）。旨在符合RFC 2710.不支持MLDv2
      ND（IPv6的邻居发现和无状态地址自动配置）。旨在符合RFC 4861（邻居发现）和RFC 4862（地址自动配置）
      DHCP，AutoIP / APIPA（Zeroconf）和（无状态）DHCPv6
      UDP（用户数据报协议），包括UDP-lite扩展
       TCP（传输控制协议）具有拥塞控制，RTT估计快速恢复/快速重传和发送SACK
      RAW/NATIVE API以提高性能
      可选的Berkeley-socket API
       TLS：可选的分层TCP（“altcp”），用于任何基于TCP的协议（移植到mbedTLS）的近乎透明的TLS（有关详细信息，请参阅changelog）
       PPPoS和PPPoE（串口/以太网上的点对点协议）
      DNS（域名解析器，包括mDNS）
      6LoWPAN（通过IEEE 802.15.4，BLE或ZEP）