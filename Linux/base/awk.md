是一种处理文本文件的语言，是一个强大的文本分析工具。

```bash
awk [选项参数] 'script' var=value file(s)
或
awk [选项参数] -f scriptfile var=value file(s)

```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240105201705.png)

```bash
# 每行按空格或TAB分割，输出文本中的1、4项
 $ awk '{print $1,$4}' log.txt


awk -F  #-F相当于内置变量FS, 指定分割字符
# 使用","分割
 $  awk -F, '{print $1,$2}'   log.txt

过滤第一列大于2的行
$ awk '$1>2' log.txt    #命令
aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$ ll
total 24K
-rwxrwxrwx 1 aaa aaa  238 Jan  5 19:15 check_sda.sh*
drwxr-xr-x 8 aaa aaa 4.0K Jan  6 22:46 dir/
-rwxrwxrwx 1 aaa aaa  229 Jan  5 19:15 for_bash.sh*
-rwxr-xr-x 1 aaa aaa  221 Jan  3 20:46 for_bash_.sh*
-rwxrwxrwx 1 aaa aaa  147 Jan  5 19:18 out_all_args.bash*
-rwxrwxrwx 1 aaa aaa  216 Jan  6 22:55 stat_dir.sh*
aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$ ll |awk '$5>220'
-rwxrwxrwx 1 aaa aaa  238 Jan  5 19:15 check_sda.sh*
drwxr-xr-x 8 aaa aaa 4.0K Jan  6 22:46 dir/
-rwxrwxrwx 1 aaa aaa  229 Jan  5 19:15 for_bash.sh*
-rwxr-xr-x 1 aaa aaa  221 Jan  3 20:46 for_bash_.sh*
aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$


过滤第一列大于2并且第二列等于'Are'的行
$ awk '$1>2 && $2=="Are" {print $1,$2,$3}' log.txt    #命令
aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$ ll |awk '$5>220 && $7==6'
drwxr-xr-x 8 aaa aaa 4.0K Jan  6 22:46 dir/


```

```bash
aaa@DESKTOP-RRJ1UGP:~/FileOP$ date +%F  |awk -F "-" '{for (i=1;i<=NF;i++)printf($i)}'
20240107


存在三个字段，说明该文件是个软链接
aaa@DESKTOP-RRJ1UGP:/usr/local/ccc$ stat -c %N ta | awk '{printf "%d\n",NF}'
3

```


```bash

root@DESKTOP-RRJ1UGP:/usr/local/ccc# stat -c %N ta | awk '{print$3}' | grep -E ".+"
'../bbb/a.txt'
root@DESKTOP-RRJ1UGP:/usr/local/ccc# stat -c %N ta | awk '{match($3, /'\''(.+)'\''/, arr); print arr[1]}'
../bbb/a.txt

match(string, regexp [, array])
/		正则表达式开始和结束标记
\'		匹配'
'\''		匹配包含单引号的字符 '
'(.+)'	匹配任意字符的捕获组

array	存储匹配的字符串

```

