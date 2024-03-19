global表示全局配置
## Timed Out
更改代理
```text
git config --global http.proxy "127.0.0.1:33210"  
git config --global https.proxy "127.0.0.1:33210"


set http_proxy=http://127.0.0.1:33210


netsh winhttp show proxy
```
**注意其中的58187端口号需要更换你自身使用的代理的端口号**

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240130184814.png)
```bash
//首先，设置默认代理，也可以理解为清除代理
git config --global --unset http.proxy
git config --global --unset https.proxy

```
## 查询配置
```bash
git config --global --list
```
## 更改配置
```text

#配置用户名
git config --global user.name "test"

#配置邮箱
git config --global user.email  abc@163.com
```
## 生成密钥
```bash
ssh-keygen -t rsa
```
