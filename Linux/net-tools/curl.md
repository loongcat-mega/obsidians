```shell
curl  url
默认是GET请求

curl -XPOST url 
发送POST请求

curl -XPOST url -d 数据（'{type:"json"}'）

curl -XPUT url -d 数据（'{type:"json"}'）
更新数据

获取报文首部
curl -I URL

将内容（png等）下载到当前文件夹，并自定义文件名为alias
curl -o alias url

恢复中断的下载
curl -C - url

跟随重定向
curl url -L

```