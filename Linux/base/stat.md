stat 以文字的格式来显示 inode 的内容


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240106230028.png)

```bash
Usage: stat [OPTION]... FILE...
Display file or file system status.

-c  --format=FORMAT   use the specified FORMAT instead of the default;
                      output a newline after each use of FORMAT
  --printf=FORMAT   like --format, but interpret backslash escapes,
                      and do not output a mandatory trailing newline;
                      if you want a newline, include \n in FORMAT


The valid format sequences for files (without --file-system):

  %a   access rights in octal (note '#' and '0' printf flags)
  %A   access rights in human readable form
aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$ stat -c %A stat_dir.sh
-rwxrwxrwx
aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$ stat -c %a stat_dir.sh
777


  %b   number of blocks allocated (see %B)
  %B   the size in bytes of each block reported by %b
  %C   SELinux security context string
  %d   device number in decimal
  %D   device number in hex
  %f   raw mode in hex
  %F   file type
aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$ stat -c %F stat_dir.sh
regular file
aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$ stat -c %F dir/
directory


  %g   group ID of owner
  %G   group name of owner
  aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$ stat -c %G dir/
aaa
  %h   number of hard links
  %i   inode number
  %m   mount point
  %n   file name
  %N   quoted file name with dereference if symbolic link
  %o   optimal I/O transfer size hint
  %s   total size, in bytes
  %t   major device type in hex, for character/block device special files
  %T   minor device type in hex, for character/block device special files
  %u   user ID of owner
  %U   user name of owner
  aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$ stat -c %U dir/
aaa


Modification time (mtime)：当该文件的『内容数据』变更时，就会更新这个时间！ 『内容数据』指的是文件中记录的内容，而不包括文件属性和权限等！

Change time (ctime)：当该文件的『状态 (status)』改变时，就会更新这个时间，举例来说， 像是文件权限、属性、inode号等被更改了，都会更新这个时间。

Access time (atime)：当我们访问该文件时，就会更新这个时间为最后一次访问该文件的时间 。 当我们使用 cat 、more、less等命令读取文件信息的时候，就会更新 atime 了
  %w   time of file birth, human-readable; - if unknown
  %W   time of file birth, seconds since Epoch; 0 if unknown
  %x   time of last access, human-readable
  %X   time of last access, seconds since Epoch
  %y   time of last data modification, human-readable
  %Y   time of last data modification, seconds since Epoch
  %z   time of last status change, human-readable
  %Z   time of last status change, seconds since Epoch
aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$ stat -c %w stat_dir.sh
-
aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$ stat -c %W stat_dir.sh
0
aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$ stat -c %x stat_dir.sh
2024-01-06 22:55:36.722684342 +0800
aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$ stat -c %X stat_dir.sh
1704552936
aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$ stat -c %y stat_dir.sh
2024-01-06 22:55:17.862687923 +0800
aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$ stat -c %z stat_dir.sh
2024-01-06 22:55:17.862687923 +0800
aaa@DESKTOP-RRJ1UGP:~/BASH_PRAC$


```