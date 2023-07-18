# 获取本地仓库

`git init`

![[Pasted image 20230717204624.png]]

# 本地仓库

![[Pasted image 20230717204942.png]]

### 查看修改的状态`git status`
### 添加工作区到暂存区`git add`
### 提交暂存区到本地仓库`git commit`
### 查看提交日志`git log`
#### --all 显示所有分支
#### --pretty=oneline 将提交的信息显示为一行
#### --abbrev-commit 使得输出的commitid更简短
#### --graph 以图的形式显示

### 版本回退`git reset --hard commitid`

回退至commitid处

### 记录版本提交和回退信息`git reflog`

### 忽略某些文件

- 新建`.gitignore` 文本
- 在该文本内利用通配符匹配不想管理的文件


## 分支

几乎所有的版本控制系统都以某种形式支持分支。使用分支意味着你可以把你的工作从开发主线上分离来进行重大的bug修改、开发新的功能，以免影响主线

head指向谁，谁就是当前分支
![[Pasted image 20230718105938.png]]


![[Pasted image 20230718114953.png]]

### 查看本地分支`git branch`
### 创建本地分支`git branch name`
### 切换分支 `git checkout name`
### 创建并切换`git check -b name`
### 合并分支`git merge name`

一个分支上的提交可以合并到另一个分支

### 删除分支 `git brach -d name`
### 强制删除分支`git brach -D name`

### 解决冲突

多个分支对同一文件同一行修改，合并时会有冲突，需手动解决

# 远程仓库

## 配置SSH公钥

### 生成SSH公钥

```txt
ssh-keygen -t rsa

一路回车
```

###  获取公钥

```txt
cat ~/.ssh/id_rsa.pub
```

### Gitee设置账户共公钥
![[Pasted image 20230718134021.png]]

## 操作远程仓库

### 远程仓库链接到本地`git remote add origin <git_ssh_url>`

### 查看远程仓库`git remote`
### 本地代码推送到远端`git push origin master`
### 克隆远端代码 `git clone <url>`
### 抓取远端代码更改`git fetch`
### 拉取远端代码更改并合并`git pull`

