能够实现断点续传

```bash
rsync -P --rsh=ssh yliu@192.168.200.2:/home/yliu/test.mp4 /root

-P：包含--progress和--partial
--rsh=ssh：使用ssh方式传输文件


alias scpr="rsync -P --rsh=ssh"
```

