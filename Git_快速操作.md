[TOC]



# 基本操作

### git clone [远程仓库地址]

这个指令将远程项目**克隆到本地**，并将远程仓库的主分支设置为本地分支的**上游**。

### git init

这个指令将**初始化**当前文件夹，在当前目录下生成 **.git** 目录，对当前文件夹进行版本控制。

### git status

这个指令将列出，当前项目下，所有文件的**状态（status）**。

### git add [文件名]

这个指令，将**unstaged**的文件加入**暂存区**，进行跟踪版本。

### git commit -m ['提交说明']

这个指令，将所有**modified**文件，提交到**本地仓库**。**comments**为提交说明。



---





# 远程管理

### git remote

列出远程仓库列表

### git remote add [仓库名] [地址]

添加一个远程仓库

### git remote remove [仓库名]

删除一个远程仓库

### git remote rename [原名] [新名]

重命名一个仓库

### git remote set-url [仓库名] [新地址]

修改仓库地址

### git remote show [仓库名]

显示某个远程仓库的信息



---





# 配置设置

### git config --global --l

列出配置列表

### git config --global core.quotepath false

设置git控制台正常输出中文

---





# 分支管理

### git branch

列出所有分支

### git branch [分支名]

新建分支

### git branch -d [分支名]

删除分支

### git branch -m [原名] [新名]

重命名分支

### git checkout [分支名]

切换分支

### git checkout  -b [分支名]

新建分支，并立即切换到该分支下。



---





# 远程仓库

### git push [仓库名] [本地分支] [远程分支]

推送本地分支到远程仓库的某一个分支。

### git push -u/--set-upstream

在第一次push时，将远程仓库作为当前分支的**upstream**（上游），以后的push操作，可不指定仓库名参数。



---





 # 版本控制

| 操作                 | 作用                       |
| :------------------- | :------------------------- |
| git log              | 查看当前分支的版本提交历史 |
| git reflog           | 查看操作日志               |
| git reset [commitID] | 回退当前分支到指定的版本。 |







