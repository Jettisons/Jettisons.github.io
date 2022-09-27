---
title: Git Notebooks
categories:
- tech
---

# GIT

## 创建版本库

创建并提交一个README.md文件

```markdown
mkdir learn_git
cd learn_git
git init
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin git@gitee.com:jettison/learn_git.git
git push -u origin master //第一次推送加上 -u，之后不用
```

## 查看状态

`git status`：可查看是否有没有提交的修改

`git diff + 文件名`：查看文件做了什么修改

`git diff + 版本号..版本号`：查看两个版本有什么区别

`git diff HEAD -- 文件名`：查看工作区和版本库里面最新版本的区别

`git log`：命令显示从最近到最远的提交日志

`git log --pretty=oneline`：简洁版e

`cat + 文件名`：查看文件内容

`git reflog`：记录操作的每一次命令

`git remote (-v)`：（详细）查看远程库的信息，若无权限则看不到push地址

## 系统配置

`git config --system -- list`：查看系统config

`git config --global  --list`：查看当前用户（global）配置

`git config --local  --list`：查看当前仓库配置信息

## 版本替换

`git reset --hard HEAD^`：回退到上一个版本

`git reset --hard + 版本号`：还原这个版本

`git checkout -- 文件名`：把文件在工作区的修改全部撤销，用版本库里版本替换工作区版本

## 分支管理

`git checkout -b 分支名称`：创建并切换到某一分支，该命令相当于

`git branch dev`+`git checkout dev`

`git branch`：查看当前分支

`git branch -r`：查看远程分支

`git branch -a`：查看所有分支

`git branch -d dev`：删除dev分支

鼓励使用switch切换分支

`git branch -D dev`：强行删除没有merge的dev分支

`git switch -c dev`：创建并切换到dev分支

`git switch 分支`：切换某分支



`git merge dev`：用于合并指定分支到当前分支

`git merge --no-ff -m "merge with no-ff" dev`：不适用快速模式合并分支



`git branch --set-upstream-to=origin/dev dev`：将本地分支与远程分支建立链接

`git rebase`：可以把本地未push的分叉提交历史整理成直线；

`git log --name-status` 每次修改的文件列表, 显示状态

`git log --name-only` 每次修改的文件列表

`git log --stat` 每次修改的文件列表, 及文件修改的统计

`git whatchanged` 每次修改的文件列表

`git whatchanged --stat` 每次修改的文件列表, 及文件修改的统计

`git show` 显示最后一次的文件改变的具体内容

`git show -5` 显示最后 5 次的文件改变的具体内容

`git show commitid` 显示某个 commitid 改变的具体内容

## 修复bug，现场保护与还原

`git stash`：可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作

`git stash list`：查看保存的工作现场

`git stash apply (列名)`：恢复现场，不删除stash内容

`git stash drop`：删除stash内容

`git stash pop`：恢复现场并删除stash内容

`git cherry-pick 提交号`：将某一提交复制到当前分支

## 删除文件

`rm 文件名`：删除文件

`git rm 文件名`：删除版本库中该文件，之后还要`git commit`

`git remote rm 库名`：删除远程库链接



## 两分支修改冲突

在A分支上修改后提交，在B分支上又修改后提交，两个分支merge的时候会发生冲突，此时会提示修改，修改完再提交即可

`git log --graph --pretty=oneline --abbrev-commit`：使用该命令可看分支历史

## 推送和抓取分支

> push

`git push <远程主机名> <本地分支名>:<远程分支名>`：完整push命令

`git push origin 分支名`：推送本地分支到存在追踪关系的远程分支，若不存在，将创建

> pull

`git pull <远程主机名> <远程分支名>:<本地分支名>`：pull命令完整格式

`git pull origin next`：远程分支与当前分支合并

`git pull origin`：若当前分支与远程分支存在追踪关系，可省略远程分支名

`git pull`：如果当前分支只有一个追踪分支，远程主机名都可以省略

如果远程主机删除了某个分支，默认情况下，`git pull` 不会在拉取远程分支的时候，删除对应的本地分支。这是为了防止，由于其他人操作了远程主机，导致`git pull`不知不觉删除了本地分支。

但是，你可以改变这个行为，加上参数 `-p` 就会在本地删除远程已经删除的分支。

```ba
$ git pull -p
# 等同于下面的命令
$ git fetch --prune origin 
$ git fetch -p
```

## 多人协作流程

当修改完文件后

首先`git add`

`git add .` ：他会监控工作区的状态树，使用它会把工作时的**所有变化提交**到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件。

`git add -u `：他仅监控**已经被add的文件**（即tracked file），他会将被修改的文件提交到暂存区。add -u 不会提交新文件（untracked file）。（git add --update的缩写）

`git add -A `：是上面两个功能的合集（git add --all的缩写）

---

然后`git commit -m '提交说明'`

多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to=origin/<branch> release` 。

这就是多人协作的工作模式，一旦熟悉了，就非常简单



`git remote add 起个名 远程库链接`：连接远程库

## 标签

`git tag`：查看所有标签

`git tag v1.0`：给当前版本打上标签v1.0

`git tag v0.9 + <commit id>`：给某一个版本打上标签

`git show + <tag name>`：查看标签信息

`git tag -a v0.1 -m "version 0.1 released" 1094adb`：创建带有说明的标签

`git push origin <tag name>`：推送某标签到服务器

`git push origin --tags`：推送全部标签到服务器

`git tag -d <tag name>`：删除标签

`git push origin :refs/tags/<tag name>`：从远程删除标签

## Git放弃修改，强制覆盖本地代码

1.`git fetch --all` //从远程拉取最新的代码 不merge

2.`git reset --hard origin/develop` //使用指定分支的代码（此处develop）强制覆盖代码

3.`git pull` //从远程拉取最新的代码 自动merge

## Git远程分支不显示问题

可执行`git config -l `命令，查看`git fetch` 的配置，上述问题可能是没有进行`git fetch`的配置（或者只配置了上游分支），可执行付下命令进行配置：

`git config remote.origin.fetch +refs/heads/*:refs/remotes/origin/*`

执行上述命令后，再进行git fetch 拉取分支，此时已包含所有远程分支。



