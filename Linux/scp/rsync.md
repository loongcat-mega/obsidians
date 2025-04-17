能够实现断点续传

[rsync](https://www.ruanyifeng.com/blog/2020/08/rsync.html)
https://www.digitalocean.com/community/tutorials/how-to-use-rsync-to-sync-local-and-remote-directories


```bash
rsync -P --rsh=ssh yliu@192.168.200.2:/home/yliu/test.mp4 /root

-P：包含--progress和--partial
--rsh=ssh：使用ssh方式传输文件


alias scpr="rsync -P --rsh=ssh"

rsync -anv -e 'ssh -p 31998' root@link-qhd-ks.lanyun.net:/root/unsharp/cases/ ./case_rsync


rsync -anv --exclude={'cases/*','esrgan/*','cases_same_scale/*','aaaaaaaaa/*'} -e 'ssh -p 31998' root@link-qhd-ks.lanyun.net:/root/unsharp/ ./unsharp_remote 



rsync -anv --exclude={'cases/*','esrgan/*','cases_same_scale/*','.venv/*','.git/*','.idea/*'} -e 'ssh -p 31998' ./unsharp  root@link-qhd-ks.lanyun.net:/root/unsharp


rsync -av -e 'ssh -e Port=6000' ./ qingruixu@182.92.170.252:C:\Users\qingruixu\ 



rsync -anv --exclude={'cases/*','cases_same_scale/*','.venv/*','.git/*','.idea/*'} -e 'ssh -p 44532' ./video_enhance  root@connect.bjc1.seetacloud.com:/root/autodl-tmp
ssh -p 44532 
```

