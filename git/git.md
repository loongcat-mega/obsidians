
`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`
3e46b2d66c9acfe1bd4c14fd32c69a73

## 配置

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

### 

## 本地仓库

在任何当前工作的 Git 仓库中，每个文件都是这样的：

-   **追踪的（tracked）**- 这些是 Git 所知道的所有文件或目录。这些是新添加（用 `git add` 添加）和提交（用 `git commit` 提交）到主仓库的文件和目录。
-   **未被追踪的（untracked）** - 这些是在工作目录中创建的，但还没有被暂存（或用 `git add` 命令添加）的任何新文件或目录。
-   **被忽略的（ignored）** - 这些是 Git 知道的要全部排除、忽略或在 Git 仓库中不需要注意的所有文件或目录。本质上，这是一种告诉 Git 哪些未被追踪的文件应该保持不被追踪并且永远不会被提交的方法。

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230717204942.png)


### 初始化本地仓库
`git init`

### 查看状态
`git status`
查看修改的状态
查看那些因包含合并冲突而处于未合并（unmerged）状态的文件：

### 将工作区文件添加到暂存区
`git add file`
添加工作区到暂存区
`git add .`  : 添加当前目录所有文件


### 将暂存区文件添加到本地仓库
`git commit`
`git commit -m str`  : 指定提交日志
`git commit -am str` ：自动将所有已修改和已删除的文件添加到暂存区，并创建一个新的提交
`git commit -i reg`  ： 添加指定规则的文件

### 修改提交信息

#### 修改最近一次的提交信息
`git commit --amend`
比如我上一次提交的是修改了某个bug，这一次我又是修改了那个bug，然后我要将这一次的修改和上一次的提交用同一个commit备注，那么你可以使用这个命令，将会使用上一次的commit备注信息，同时生成一个新的commitId，
你想把本次的修改提交到上一次的提交中，并且把上一次备注的提交信息改成这次的

#### 修改某一次的提交信息
`git rebase -i master^^`
它的排列顺序是一个正序排序，也就是说旧的commit信息在上面，新的commit在下面
将pick改为edit
然后`git commit --amend	`去修改提交记录
改完信息后，我们还需要`git rebase --continue`，将基准从当前倒数第二位置移到最新一次提交
在 commit 的后面加一个或多个 ^ 号，可以把 commit 往回偏移，偏移的数量是 ^ 的数量。例如：master^^表示 当前master 指向的 commit 之前倒数第2个 commit


### 储藏修改

`git stash`
把所有未提交的修改（包括暂存的和非暂存的）都保存起来，用于后续恢复当前工作目录
`git stash save`取代`git stash`命令

默认情况下，`git stash`会缓存下列文件：

-   添加到暂存区的修改（**staged changes**）
-   Git跟踪的但并未添加到暂存区的修改（**unstaged changes**）

但不会缓存以下文件：

-   在工作目录中新的文件（**untracked files**）
-   被忽略的文件（**ignored files**）

可以通过`git stash pop`命令恢复之前缓存的工作目录
`git stash apply`命令时可以通过名字指定使用哪个**stash**，默认使用最近的stash

`git stash drop`   移除stash
`git stash clear`命令，删除所有缓存的stash。

### 删除文件

`rm/git rm`

#### 删除工作区文件

`rm file`
rm 命令只是删除工作区的文件，并没有删除版本库的文件
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240316095728.png)

#### 删除工作区文件并将删除的文件添加到暂存区
`git rm file`
删除工作区文件，并且将这次删除放入暂存区
要删除的文件是没有修改过的，就是说和当前版本库文件的内容相同
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240316095809.png)


### 删除暂存区中的文件,保留工作区中的文件,并将此次删除提交到暂存区
`git rm --cache file`
删除暂存区的文件，但是不删除本地文件，(git add file的东西)

### 查看提交日志
`git log`
查看提交日志
#### --all 显示所有分支
#### --pretty=oneline 将提交的信息显示为一行
#### --abbrev-commit 使得输出的commitid更简短
#### --graph 以图的形式显示

### 版本回退

`git reset `

```bash
git reset --soft
–-soft  
不删除工作空间的改动代码 ，撤销commit，不撤销git add file
如果你进行了2次commit，想都撤回，可以使用HEAD~2

–-hard  
删除工作空间的改动代码，撤销commit且撤销add
你的缓存区和工作目录里的内容会被完全重置为和HEAD的新位置相同的内容。换句话说，就是你的没有commit的修改会被全部擦掉

--mixed
保留工作目录，并清空暂存区。也就是说，工作目录的修改、暂存区的内容以及由 reset 所导致的新的文件差异，都会被放进工作目录
```
#### git reset --hard commitid

版本回退
回退至commitid处




### 查看版本提交和回退记录

`git reflog`


### 忽略某些文件

- 新建`.gitignore` 文本
- 在该文本内利用通配符匹配不想管理的文件

你不需要提供特定文件的完整路径。这种模式将忽略位于项目中任何地方的具有该特定名称的所有文件
要忽略整个目录及其所有内容，你需要包括目录的名称，并在最后加上斜线 `/`：

```bash
test/
```

这个命令将忽略位于你的项目中任何地方的名为 `test` 的目录（包括目录中的其他文件和其他子目录）。

需要注意的是，如果你只写一个文件的名字或者只写目录的名字而不写斜线 `/`，那么这个模式将同时匹配任何带有这个名字的文件或目录：

```bash
# 匹配任何名字带有 test 的文件和目录
test
```

不希望 Git 忽略一个 `README.md` 文件。

要做到这一点，你需要使用带有感叹号的否定模式，即 `!`，来排除一个本来会被忽略的文件：

```bash
# 忽略所有 .md 文件
.md

# 不忽略 README.md 文件
!README.md


# 忽略所有名字带有 test 的目录
test/

# 试图在一个被忽略的目录内排除一个文件是行不通的
!test/example.md
```



## 分支

几乎所有的版本控制系统都以某种形式支持分支。使用分支意味着你可以把你的工作从开发主线上分离来进行重大的bug修改、开发新的功能，以免影响主线
head指向谁，谁就是当前分支

### 查看本地分支

`git branch`

### 创建本地分支
`git branch name`

### 切换分支
`git checkout name`

### 创建并切换
`git checkout -b name`

### 重命名分支
`git branch -m <oldbranch> <newbranch> `

### 删除分支
`git brach -d name`
`git brach -D name`  :  强制删除分支


### 分支合并
`git merge name`
合并分支
一个分支上的提交可以合并到另一个分支
#### 无冲突
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240316090011.png)


如果在B2提交对象处新建一个分支dev，并且对dev进行了修改，master并没有修改，那么切换回master分支，并执行merge dev命令后，会直接合并对象，等效于master指针移动到dev处
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240316090136.png)

#### 有冲突(同一文件不同部分/不同文件/同一文件同一部分)
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240316090403.png)

在B2对象处新建分支dev，并且master和dev都有修改(提交),这是merge会把B3 B4 以及二者共同祖先B2进行一个**三方合并**

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240316090557.png)


### 变基
`gir rebase name`

从B对象处新增一个分支feature，同时master有feature分支都有新提交
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240316092024.png)
此时切换回feature分支，执行变基命令
```git
git checkout feature
git rebase master
```

feature为待变基分支，master为基分支
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240316092224.png)


### git rm
删除仓库内文件

### 解决冲突

多个分支对同一文件同一行修改，合并时会有冲突，需手动解决




## 远程仓库

### 连接远程仓库  
`git remote add origin url`


### 查看远程连接
`git remote -v`

### 显示指定远程仓库的详细信息
`git remote show origin`
显示指定远程仓库的详细信息,包括 URL 和跟踪分支

### 取消与远程仓库的连接  
`git remote remove origin`

### 修改仓库名
`git remote rename old_name new_name`





## 与远程仓库交互

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231229225121.png)



### 克隆远端代码
`git clone url`

### 拉取远端代码更改并合并
`git pull`

### 本地代码推送到远端
`git push origin master`


### 抓取远端代码更改
`git fetch`

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240316094215.png)


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
