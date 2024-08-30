![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240320110245.png)

`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`
3e46b2d66c9acfe1bd4c14fd32c69a73
branch 通常用于开发、调试和合并代码，而 tag 则用于发布和版本控制

## 协议

[服务器上的 Git - 协议](https://git-scm.com/book/zh/v2/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E5%8D%8F%E8%AE%AE#Popover19-toggle:~:text=%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84%20Git%20-%20%E5%8D%8F%E8%AE%AE)

### 本地协议local protocol
其中的远程版本库就是同一主机上的另一个目录。 这常见于团队每一个成员都对一个共享的文件系统（例如一个挂载的 NFS）拥有访问权
克隆一个版本库或者增加一个远程到现有的项目中，使用版本库路径作为 URL
`git clone /srv/git/project.git`
或
`git clone file:///srv/git/project.git`

要增加一个本地版本库到现有的 Git 项目，可以执行如下的命令
```console
git remote add local_proj /srv/git/project.git
```
### http 协议
智能 HTTP 协议或许已经是最流行的使用 Git 的方式了，它既支持像 `git://` 协议一样设置匿名服务， 也可以像 SSH 协议一样提供传输时的授权和加密。 而且只用一个 URL 就可以都做到，省去了为不同的需求设置不同的 URL。
如果你要推送到一个需要授权的服务器上（一般来讲都需要），服务器会提示你输入用户名和密码。 从服务器获取数据时也一样。

### ssh协议

SSH 协议的缺点在于它不支持匿名访问 Git 仓库。 如果你使用 SSH，那么即便只是读取数据，使用者也 **必须** 通过 SSH 访问你的主机， 这使得 SSH 协议不利于开源的项目，毕竟人们可能只想把你的仓库克隆下来查看。
### git协议

俺们是乡下人，看不懂



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

### 别名
```console
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.sh stash
```

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

### 追溯历史修改的内容

`git blame fn`
查看文件最近一次修改记录

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240321190407.png)

`git blame -L 69,82 Makefile		查看Makefile这个文件第69--82行最近一次的修改记录`


![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240321150616.png)
`git show` 默认展示最近一次提交的修改
`git show` 还可以加数字，比如 `git show -3` 就是展示最近3次提交修改信息

`git show ciid fname`
查看某次提交的文件内容修改
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240321190213.png)


### 附注标签
```text
git tag {标签名} {提交ID}
```
在Git中，标签（tag）是一个特别的分支，指向某个提交（commit）。它**通常用于发布版本**。
Git的标签分为两种类型：轻量标签和附注标签

```text
git tag -a v1.0.0 -m "Release version 1.0.0" HEAD
```
默认情况下，`git push`命令不会将标签推送到远程服务器，需要使用以下命令将标签推送到远程服务器：
```text
git push origin {标签名}
```
如果要一次性推送所有本地标签，可以使用以下命令：
```text
git push origin --tags
```
删除本地标签的命令如下：
```text
git tag -d {标签名}
```
删除远程标签的命令如下：
```text
git push origin :refs/tags/{标签名}
```

### 储藏修改

有时，当你在项目的一部分上已经工作一段时间后，所有东西都进入了混乱的状态， 而这时你想要切换到另一个分支做一点别的事情。 问题是，你不想仅仅因为过会儿回到这一点而为做了一半的工作创建一次提交。 针对这个问题的答案是 `git stash` 命令。
贮藏（stash）会处理工作目录的脏的状态——即跟踪文件的修改与暂存的改动——然后将未完成的修改保存到一个栈上， 而你可以在任何时候重新应用这些改动（甚至在不同的分支上）。
把所有未提交的修改（包括暂存的和非暂存的）都保存起来，用于后续恢复当前工作目录

`git stash save`取代`git stash`命令

默认情况下，`git stash`会缓存下列文件：

-   添加到暂存区的修改（**staged changes**）
-   Git跟踪的但并未添加到暂存区的修改（**unstaged changes**）

但不会缓存以下文件：

-   在工作目录中新的文件（**untracked files**）
-   被忽略的文件（**ignored files**）

如果指定 `--include-untracked` 或 `-u` 选项，Git 也会贮藏任何未跟踪文件。 然而，在贮藏中包含未跟踪的文件仍然不会包含明确 **忽略** 的文件。 要额外包含忽略的文件，请使用 `--all` 或 `-a` 选项。

如果贮藏了一些工作，将它留在那儿了一会儿，然后继续在贮藏的分支上工作，在重新应用工作时可能会有问题。 如果应用尝试修改刚刚修改的文件，你会得到一个合并冲突并不得不解决它。 如果想要一个轻松的方式来再次测试贮藏的改动，可以运行 `git stash branch <new branchname>` 以你指定的分支名创建一个新分支，检出贮藏工作时所在的提交，重新在那应用工作，然后在应用成功后丢弃贮藏：
`git stash branch <branch-name> stash@{<stash-index>}`
相当于把某次暂存变为分支，并从暂存列表中剔除

 查看某个stash具体内容 && 比较stash与当前工作目录
```linux
git stash show -p stash@{3}
```
Git 的储藏功能是针对工作目录中的更改，而不是针对特定分支的。因此，你可以在任何分支上使用任何已经保存的储藏。

可以通过`git stash pop`命令恢复之前缓存的工作目录
`git stash apply`命令时可以通过名字指定使用哪个**stash**，默认使用最近的stash

`git stash drop`   移除stash
`git stash clear`命令，删除所有缓存的stash。
### 文件重命名
`git mv old new`
等价于
```git
mv old new
git rm old
git add new
```

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


```git
git log --all --pretty=oneline --abbrev-commit --graph 
```
#### --all 显示所有分支
#### --pretty=oneline 将提交的信息显示为一行
#### --abbrev-commit 使得输出的commitid更简短
#### --graph 以图的形式显示



或许你不想知道某一项在 **哪里** ，而是想知道是什么 **时候** 存在或者引入的。 `git log` 命令有许多强大的工具可以通过提交信息甚至是 diff 的内容来找到某个特定的提交。

例如，如果我们想找到 `ZLIB_BUF_MAX` 常量是什么时候引入的，我们可以使用 `-S` 选项 （在 Git 中俗称“鹤嘴锄（pickaxe）”选项）来显示新增和删除该字符串的提交。
```console
git log -S ZLIB_BUF_MAX --oneline
```
行日志搜索是另一个相当高级并且有用的日志搜索功能。 在 `git log` 后加上 `-L` 选项即可调用，它可以展示代码中一行或者一个函数的历史。

例如，假设我们想查看 `zlib.c` 文件中 `git_deflate_bound` 函数的每一次变更， 我们可以执行 `git log -L :git_deflate_bound:zlib.c`。 Git 会尝试找出这个函数的范围，然后查找历史记录，并且显示从函数创建之后一系列变更对应的补丁。
```console
git log -L :git_deflate_bound:zlib.c
```

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
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240320102318.png)
许多使用 Git 的开发者都喜欢使用这种方式来工作，比如只在 `master` 分支上保留完全稳定的代码——有可能仅仅是已经发布或即将发布的代码。 他们还有一些名为 `develop` 或者 `next` 的平行分支，被用来做后续开发或者测试稳定性——这些分支不必保持绝对稳定，但是一旦达到稳定状态，它们就可以被合并入 `master` 分支了。 这样，在确保这些已完成的主题分支（短期分支，比如之前的 `iss53` 分支）能够通过所有测试，并且不会引入更多 bug 之后，就可以合并入主干分支中，等待下一次的发布。
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
### 删除远端分支

`git push origin --delete name`

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


首先，在做一次可能有冲突的合并前尽可能保证工作目录是干净的。 如果你有正在做的工作，要么提交到一个临时分支要么储藏它。 这使你可以撤消在这里尝试做的 **任何事情** 。 如果在你尝试一次合并时工作目录中有未保存的改动，下面的这些技巧可能会使你丢失那些工作。

空白行冲突：每一行都被移除而在另一边每一行又被加回来了。 默认情况下，Git 认为所有这些行都改动了，所以它不会合并文件。

默认合并策略可以带有参数，其中的几个正好是关于忽略空白改动的。 如果你看到在一次合并中有大量关于空白的问题，你可以直接中止它并重做一次， 这次使用 `-Xignore-all-space` 或 `-Xignore-space-change` 选项。 第一个选项在比较行时 **完全忽略** 空白修改，第二个选项将一个空白符与多个连续的空白字符视作等价的。

一个很有用的工具是带 `--conflict` 选项的 `git checkout`。 这会重新检出文件并替换合并冲突标记。 如果想要重置标记并尝试再次解决它们的话这会很有用。

可以传递给 `--conflict` 参数 `diff3` 或 `merge`（默认选项）。 如果传给它 `diff3`，Git 会使用一个略微不同版本的冲突标记： 不仅仅只给你 “ours” 和 “theirs” 版本，同时也会有 “base” 版本在中间来给你更多的上下文。

```console
git checkout --conflict=diff3 fn
```

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

### 修改remote URL
`git remote set-url命令`

```text
git remote set-url传递两个参数

-   remote name。例如，origin或者upstream
-   new remote url。例如，git@github.com:USERNAME/OTHERREPOSITORY.git
```
可以从https认证改为ssh认证
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240319232056.png)


## 与远程仓库交互

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240316094215.png)

尽管在技术上你可以从个人仓库进行推送（push）和拉取（pull）来修改内容，但不鼓励使用这种方法，因为一不留心就很容易弄混其他人的进度

### 克隆远端代码
`git clone url`

### 拉取远端代码更改并合并
`git pull`

### 本地代码推送到远端
`git push origin master`


### 抓取远端代码更改
`git fetch`

拉取之后 像这样
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240320105428.png)

```console
git merge origin/master
```
合并之后像这样
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240320105520.png)


# Usage
[分布式 Git - 向一个项目贡献](https://git-scm.com/book/zh/v2/%E5%88%86%E5%B8%83%E5%BC%8F-Git-%E5%90%91%E4%B8%80%E4%B8%AA%E9%A1%B9%E7%9B%AE%E8%B4%A1%E7%8C%AE#Popover19-toggle:~:text=%E5%88%86%E5%B8%83%E5%BC%8F%20Git%20-%20%E5%90%91%E4%B8%80%E4%B8%AA%E9%A1%B9%E7%9B%AE%E8%B4%A1%E7%8C%AE)

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
