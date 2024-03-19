global regular expression 用于查找文件里符合条件的字符串或正则表达式


```text
grep [options] pattern [files]
patten 		要查找的字符串或正则表达式
files		要查找的文件名，可以同时查找多个文件，若缺省，则默认从标准输入中读取数据
[options]:
-i			忽略大小写
—v			反向查找
-n           显示匹配的行号
-l           只打印匹配的文件名
-c           只打印匹配的行数
-E           将样式延伸为正则表达式


或
grep [-abcEFGhHilLnqrsvVwxy][-A<显示行数>][-B<显示列数>][-C<显示列数>][-d<进行动作>][-e<范本样式>][-f<范本文件>][--help][范本样式][文件或目录...]

查看关键字前面3行，后面5行
 grep -B 3 -A 5 "第 0 个机器人" cerr.txt
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240105194513.png)


