脚本程序都是由解释器来执行的

- shell 脚本解释器  /bin/sh
- Python 脚本解释器  /bin/python3

```shell
#!/bin/sh

#! 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell
```

## 基本$

### $@


当有双引号时,`"$@"`表示输入的每个参数
```bash
#!/bin/bash
for i in "$@"
do
    echo $i
done
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240105190756.png)





### `$#`

获得参数列表的总个数
```bsh
#!/bin/bash
echo 共有$#个参数
for i in "$@"
do
    echo $i
done
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240105191107.png)



### `$$ $! $?`

```bash
$$ 	获得当前进程ID
$! 	获得上一进程ID
$?	获得上一进程结束的状态码 (0表示成功 1表示失败)
```

### ` `` 与$`
```bash
for file in `ls /etc`  
==
for file in $(ls /etc)
```

### $n
脚本内获取参数的格式为`$n` ,n代表一个数字，如`$1` 表示第一个参数，`$2`表示第二个参数
`$0` 代表执行的文件名

```bash
#!/bin/bash
echo 共有$#个参数

echo 执行的文件名 $0
echo 第一个参数 $1
echo 第二个参数 $2

for i in "$@"
do
    echo $i
done
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240105191918.png)
## ops

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。
```bash
#!/bin/bash

val=`expr 2 + 2`
echo "两数之和为 : $val"
```
ops 与 expr之间要有空格
条件表达式要放在方括号之间，并且要有空格，例如: `[$a==$b] 是错误的，必须写成 [ $a == $b ]

```bash
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

a=10
b=20

val=`expr $a + $b`
echo "a + b : $val"

val=`expr $a - $b`
echo "a - b : $val"

val=`expr $a \* $b`
echo "a * b : $val"

val=`expr $b / $a`
echo "b / a : $val"

val=`expr $b % $a`
echo "b % a : $val"

if [ $a == $b ]
then
   echo "a 等于 b"
fi
if [ $a != $b ]
then
   echo "a 不等于 b"
fi
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240120175040.png)
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240120175024.png)
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240120175133.png)
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240120175147.png)
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240120175318.png)


## 流程控制

### if
```bash
if condition
then
    command1 
    command2
    ...
    commandN 
fi

or

if [ $(ps -ef | grep -c "ssh") -gt 1 ]; then echo "true"; fi

使用 ((...)) 作为判断语句
if (( $a == $b ))  
then  
   echo "a 等于 b"
fi
```
## prac

### for

写一个脚本 遍历 /data目录下的txt文件，将这些txt文件做一个备份，备份的文件名增加一个年月日的后缀
```bash
#!/bin/bash

## 定义后缀
suffix=$(date '+%Y%m%d')

## 找到指定目录的txt文件，用for循环遍历
for f in $(find ~/BASH_PRAC/ -type f -name "*.txt")
do
    echo "备份文件 $f"
    cp "${f}" "${f}_${suffix}"
done



```

```bash
#!/bin/bash

## 定义后缀
suffix=`date +%Y%m%d`

## 找到指定目录的txt文件，用for循环遍历
for f in `find ~/BASH_PRAC/ -type f -name "*.txt"`
do
    echo "备份文件 $f"
    cp ${f} ${f}_${suffix}
done
```

### $应用

检测标本：检测本机所有磁盘分区读写是否都正常
想法：在每块磁盘分区`touch`一个文件，然后再删除，看读写是否正常

```bash
#!/bin/bash
##遍历每块分区
for mount_p in `df |sed '1d' |grep -v 'tmpfs' | awk '{print $NF}'`
do
        touch $mount_p/testfile && rm -f $mount_p/testfile
        ## 进程退出值
        if [$? -ne 0]
        then
                echo "$mount_p 读写有问题"
        else
                echo "$mount_p 读写正常"
        fi
done
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240105201607.png)


### sort

将当前目录的文件按大小/按日期排序

按大小排序:
```bash
#!/bin/sh
if [ $# -eq 0 ]
then
        ls -l | sort -nrk 5
elif [ "$1" = "-r" ]
then
        ls -l | sort -nk 5
fi
```

按日期排序:
```bash
按修改日期降序排序(最新的在最前)
ll -t		

按修改日期降序排序(最新的在最后)
ll -tr

```


### stat

检查`/home/aaa/BASH_PRAC/dir`目录下所有文件和目录，查看是否满足以下条件：
- 所有文件权限为644
- 所有目录权限为755
- 文件和目录所有者为www，所属组为root
要有判断的过程

```bash
#!/bin/bash
cd /home/aaa/BASH_PRAC/dir
for f in `find .`
do
        f_p=`stat -c %a $f`

        ##判断是否是目录
        if [ -d $f ]
        then
        			##只有当&&前面命令执行成功才会执行后面语句
                [ $f_p != '755' ]  && chmod 755 $f
        else
                [ $f_p != '644' ]  && chmod 644 $f
        fi
done
```


### main 与 rsync

有一个目录`data/att` 该目录下有数百个子目录，比如`/data/att/linux`，然后再深入一层是以日期为命名的目录，例如`/data/att/linux/20240107`，每天会生成一个日期新目录，由于`/data`所在磁盘快满了，所以需要将老文件(一年前)挪到一个新的目录`data1/att`下，挪完之后，还需要做软链接，脚本每天一点会执行一次，需要有输出日志


```bash
#!/bin/bash
main()
{
        cd /data/att
        for dir in `ls`
        do
                for dir2 in `find $dir -maxdepth 1 -type d -mtime +365`
                do
                        rsync -aR $dir2/ /data1/att
                        if [ $? != 0 ]
                                rm -rf $dir2
                                ln -s /data1/att/$dir2 /data/att/$dir2 && \
                                        echo "成功/data/att/$dir2成功创建软 链接"

                        else
                                echo "/data/att/$dir2未移动成功"
                        fi
                done
        done

}
## 调用main函数
main &> /tmp/move_old_data_`date +%F`.log
```
`&>` 将标准输出和标准错误都重定位到某个地方


```shell
实时显示时间
watch -n 1 date
```


### 链接文件

将被链接文件名改为链接文件的名
```bash
#!/bin/bash
## 将当前目录的文件传递到数组中
readarray -t linksname <<< "$(ls)"
## 遍历数组
for ((i=0; i<${#linksname[@]}; i++)); do
		## 判断是否是链接文件
    if [ "$(stat -c %F "${linksname[i]}")" == "symbolic link" ]; then
    		## 获取被链接文件
        link_path=$(readlink -f "${linksname[i]}")
        ## 获取被链接文件路径
        directory=$(dirname "${link_path}")
        ## 将路径与链接文件名组合
        path="$directory/${linksname[i]}"
        mv "$link_path" "$path"
    fi
done

```
