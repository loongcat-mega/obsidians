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



lsof -i:8080：查看8080端口占用
lsof abc.txt：显示开启文件abc.txt的进程
lsof -c abc：显示abc进程现在打开的文件
lsof -c -p 1234：列出进程号为1234的进程所打开的文件
lsof -g gid：显示归属gid的进程情况
lsof +d /usr/local/：显示目录下被进程开启的文件
lsof +D /usr/local/：同上，但是会搜索目录下的目录，时间较长
lsof -d 4：显示使用fd为4的进程
lsof -i -U：显示所有打开的端口和UNIX domain文件
```


