
`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`

# 初始化本地仓库

`git init`


# 本地仓库

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230717204942.png)

### git status
查看修改的状态
### git add 
添加工作区到暂存区

###  git commit
提交暂存区到本地仓库
###  git log
查看提交日志
#### --all 显示所有分支
#### --pretty=oneline 将提交的信息显示为一行
#### --abbrev-commit 使得输出的commitid更简短
#### --graph 以图的形式显示

### git reset --hard commitid

版本回退
回退至commitid处
```bash
git reset --soft
–-soft  
不删除工作空间的改动代码 ，撤销commit，不撤销git add file

–-hard  
删除工作空间的改动代码，撤销commit且撤销add
```


### git reflog
记录版本提交和回退信息

### 忽略某些文件

- 新建`.gitignore` 文本
- 在该文本内利用通配符匹配不想管理的文件


## 分支

几乎所有的版本控制系统都以某种形式支持分支。使用分支意味着你可以把你的工作从开发主线上分离来进行重大的bug修改、开发新的功能，以免影响主线

head指向谁，谁就是当前分支
### git branch
查看本地分支
### git branch name
创建本地分支
###  git checkout name
切换分支
### git checkout -b name
创建并切换
### git merge name
合并分支

一个分支上的提交可以合并到另一个分支

###  git brach -d name
删除分支
### git brach -D name
强制删除分支

### git rm
删除仓库内文件

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

## 操作远程仓库

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231229225121.png)

### git remote rename old_name new_name
修改仓库名
### git remote add origin <git_ssh_url>
远程仓库链接到本地
### git remote
列出当前仓库中已配置的远程仓库
```bash
git remote -v
列出当前仓库中已配置的远程仓库，并显示它们的 URL

git remote remove <remote_name>
从当前仓库中删除指定的远程仓库

git remote show origin
显示指定远程仓库的详细信息，包括 URL 和跟踪分支
```
### git push origin master
本地代码推送到远端
###  git clone url
克隆远端代码
### git fetch
抓取远端代码更改
### git pull
拉取远端代码更改并合并

# 利用 lfs上传大文件

[ Download v3.3.0 (Windows)](https://git-lfs.com/#Popover19-toggle:~:text=%20Download%20v3.3.0%20%28Windows%29)

## 下载安装

```git
git lfs install

git lfs track "*.zip"

```
```
git add .gitattributes
git commit -m ""
```
## 问题

### 报错1

```
WARNING: Authentication error: Authentication required: LFS only supported repository in paid enterprise.
batch response: LFS only supported repository in paid enterprise.
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230724210214.png)
### 解决方案
```
git config lfs.https://gitee.com/{your_gitee}/{your_repo}.git/info/lfs.locksverify false

```
### 报错2

```
batch response: LFS only supported repository in paid enterprise.
```


### 解决方案
```
rm .git/hooks/pre-push
```
# Usage

```bash
1.git clone // 到本地  
2.git checkout -b xxx 切换至新分支xxx  
（相当于复制了remote的仓库到本地的xxx分支上  
3.修改或者添加本地代码（部署在硬盘的源文件上）  
4.git diff 查看自己对代码做出的改变  
5.git add 上传更新后的代码至暂存区  
6.git commit 可以将暂存区里更新后的代码更新到本地git  
7.git push origin xxx 将本地的xxxgit分支上传至github上的git  
-----------------------------------------------------------  
（如果在写自己的代码过程中发现远端GitHub上代码出现改变）  
1.git checkout main 切换回main分支  
2.git pull origin master(main) 将远端修改过的代码再更新到本地  
3.git checkout xxx 回到xxx分支  
4.git rebase main 我在xxx分支上，先把main移过来，然后根据我的commit来修改成新的内容  
（中途可能会出现，rebase conflict -----》手动选择保留哪段代码）  
5.git push -f origin xxx 把rebase后并且更新过的代码再push到远端github上  
（-f ---》强行）  
6.原项目主人采用pull request 中的 squash and merge 合并所有不同的commit  
----------------------------------------------------------------------------------------------  
远端完成更新后  
1.git branch -d xxx 删除本地的git分支  
2.git pull origin master 再把远端的最新代码拉至本地
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231220103511.png)


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231220103624.png)


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231220103808.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231220104013.png)


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231220103953.png)
