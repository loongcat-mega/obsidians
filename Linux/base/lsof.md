lsof （list open files) 是一个列出当前系统打开文件的工具。

```bash
列举已经打开的网络文件
lsof -i

列举在指定端口打开的文件
lsof -i:port_num

列出使用了指定协议(TCP/UDP) 的文件
lsof -i TCP
lsof -i UDP

使用 lsof -i TCP:1-1024 列出使用了TCP协议并且端口范围为 1 到 1024 的文件

列出指定进程ID打开的文件
lsof -p pid1,pid2

```


