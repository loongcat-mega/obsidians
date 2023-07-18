
输入`ftp`进入ftp模式

`open ip` 进入ftp服务器


## 文件操作

`dir` 查看当前ftp目录的文件
`cd` 来切换ftp系统目录。
`mkdir` 来新建一个目录（文件夹）。
`delete 路径+文件名` 来删除文件。
`rm 路径名` 来删除文件夹。
`lcd`  设置当前用户工作路径，也就是要把资源下载到本地哪个文件夹。
`pwd` 命令查看当前路径。


## 上传文件 put send

```shell

put E:\test.txt
send E:\test.txt
mput E:\test.txt E:\test1.txt

```

## 下载文件

```shell
get ./test.txt
mget ./test.txt ./test1.txt

```
## 断开连接


`bye`


