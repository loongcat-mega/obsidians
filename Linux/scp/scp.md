
scp命令用于Linux之间复制文件和目录，scp是secure copy的缩写，是基于ssh登录进行安全的远程文件拷贝命令
```bash
scp [可选参数] file_source file_target 
```
### 从本地复制到远程
```bash
scp local_file remote_username@remote_ip:remote_folder 
或者 
scp local_file remote_username@remote_ip:remote_file 
或者 
scp local_file remote_ip:remote_folder 
或者 
scp local_file remote_ip:remote_file 

将当前的目录递归拷贝到远程服务器md/目录
scp -r ./mindspore_md/ root@47.95.4.132:~/md
```

### 从远程复制到本地

把上面两个参数对调
```bash
将远程服务器的文件递归复制到当前目录，使用端口51371
scp -r -P 51371 root@region-45.autodl.pro:~/autodl-tmp/mindspore_md ./
```
