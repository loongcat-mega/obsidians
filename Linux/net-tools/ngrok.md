
`choco install ngrok`

Run the following command to add your authtoken to the default **ngrok.**

```
ngrok config add-authtoken 2ZQuNyn7M7gQOMLdtVIFcSCHAzL_88JRBpyUsfDnSb9qsVTGr
```


```
ngrok http 80
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231231184147.png)

此时TCP代理开启，通过ngrok代理的虚拟主机地址为`0.tcp.ngrok.io`，端口号为：`13632`。

