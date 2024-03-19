可依照脚本的指令来处理、编辑文本文件。
`sed [-hnV][-e<script>][-f<script文件>][文本文件]`

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240105200135.png)

## p

查看起始行到结束行

```bash
sed -n '1,10p' file
```
## a

在第二行后插入 newline
```bash
nl file | sed '2a newline'
```

## i

在第二行前插入 newline
```bash
nl file | sed '2i newline'
```

## d
```bash
删除第二行
nl file | sed '2d'

删除2-5行
nl file | sed '2,5d'

删除2-末行
nl file | sed '2-$d'

在文件file中，删除含有字符串str的行
sed -i '/str/d' file

```

## c


```bash
将第二行整行替换为newline
nl file | sed '2c newline'
```

## s

```bash

sed 's/要被取代的字串/新的字串/g'

将文件中所有oo替换为xx
nl file | sed 's/oo/xx/g'

此修改不会修改源文件
sed -e 's/oo/kk/' testfile
此修改会修改源文件
sed -i 's/oo/kk/g' testfile
```


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240105201122.png)

