[超详细！我的世界/MC服务器搭建入门教程](https://zhuanlan.zhihu.com/p/508274523#Popover19-toggle:~:text=%E8%B6%85%E8%AF%A6%E7%BB%86%EF%BC%81%E6%88%91%E7%9A%84%E4%B8%96%E7%95%8C%2FMC%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%90%AD%E5%BB%BA%E5%85%A5%E9%97%A8%E6%95%99%E7%A8%8B)

## mc	官方服务端
https://link.zhihu.com/?target=https%3A//mcversions.net/

## 开服

```bash
java -jar server.jar nogui
vim eula.txt
将其中的eula = false 改为eula = true。


```


## 使用Screen

可能有的同学会担心，关闭ssh窗口后服务器会不会继续运行。对此，linux很安全，虽然不会把mc的服务端进程杀掉，但是我们也不能再回到服务器后台的界面中。如果你不幸遇到了这种情况，请这样操作来先把服务端杀掉：

```bash
screen -S 一个名字
```
这个操作的意思是创建一个新的screen，并把-S后的内容作为它的名字

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312141820147.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312142239432.png)
