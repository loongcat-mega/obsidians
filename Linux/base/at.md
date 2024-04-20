定时完成任务

```
at [选项] [日期时间]

-f：指定包含具体指令的任务文件
-q：指定新任务的队列名称
-l：显示待执行任务的列表
-d：删除指定的待执行任务
-m：任务执行完成后向用户发送 E-mail



at 命令使用的是我们日常生活中所使用的时间格式，非常方便：

-   YYMMDDhhmm[.ss]  
    （缩写年、月、日、小时、分钟[秒]）
-   CCYYMMDDhhmm[.ss]  
    （完整年、月、日、小时、分钟和[秒]）
-   now
-   midnight
-   noon
-   teatime`（下午4点）
-   AM
-   PM

时间和日期可以是绝对的，也可以添加一个加号 ( _+_ ) 使它们相对于_现在_。在指定相对时间时，下面这些日常生活中所使用的词汇都可以使用：

-   minutes
-   hours
-   days
-   weeks
-   months
-   years

```

定时任务
```text

五个小时定时关机
at now +5hours
>shutdown -h
```

```bash
echo 'hello again' >> ~/log.txt | at now +1 minute

```
