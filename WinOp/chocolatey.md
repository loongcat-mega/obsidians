Chocolatey是一个适用于windows操作系统的软件管理工具，同时也是一个包管理器。他类似于Linux上使用的包管理器，例如apt yum等。choco的目的是简化windows上的软件的安装、升级、配置和卸载过程。

特点：
- 简化软件管理：通过命令行操作
- 自动化
- 版本控制
- 快速更新
- 软件仓库


## 与pip的比较

pip只使用与pyhon环境，Chocolatey 不仅限于特定语言或环境，而是用于整个Windows系统的软件包管理



## 安装

```powershell
Set-ExecutionPolicy AllSigned

Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312182234956.png)

## 使用

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312182242132.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312182244621.png)

- 列出本地已安装的包

```bash
choco list –local-only
```
- 显示已安装包信息

```bash
choco info <package-name>
```

- 安装本地包

```bash
choco install <package-name> --source="'c:\path\to\directory'"
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312182251143.png)

