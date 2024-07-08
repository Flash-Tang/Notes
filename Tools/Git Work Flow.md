# Git Work Flow

## Basic

+ 查看版本库状态`git status`
+ 添加文件到暂存区：`git add file1, file2`
+ 提交暂存区文件到版本库：`git commit -m "commit message"`

## Start point

+ ```shell
  # 1. 本地新建分支方式
  # 切换到主分支
  git switch master
  # 拉取最新代码
  git pull
  # 在本地创建branchName分支，并切换到该分支
  git checkout -b branchName
  # 把本地分支推送到远程仓库并将本地分支与远程分支相关联
  git push -u origin <branch_name>
  #########################################################
  
  # 2. 网页端新建分支方式
  # 网页端新建branchName分支
  # 拉取刚刚创建的新分支
  git pull
  # 在本地创建和远程分支对应的分支
  git checkout -b branch-name origin/branch-name
  ```
  
  

## Branch

+ 创建并切换到分支：`git checkout -b <branchName>` 或 `git switch -c <branchName>`
+ 切换分支：`git checkout <branchName>` 或 `git switch <branchName>`
+ 删除分支：`git branch -d <branchName>`
+ 合并`branchName`分支到当前分支：`git merge <branchName>`

## Push & Pull

+ 推送本地分支到远程：`git push origin <branchName>`
+ 获取远程新的提交并合并分支：
  + 合并到当前分支：`git pull origin 远程分支名`
  + 合并到指定分支：`git pull origin 远程分支名:本地分支名`
  + 关联了远程分支时：`git pull`
  + `git pull` is shorthand for `git fetch` + `git merge`


## Remote

+ Push an existing folder：
  + 初始化本地库：`git init`
  + 添加远程库，origin表示远程库：`git remote add origin <url>`
  + 第一次推送加-u(--set-upstream)参数以关联远程分支：`git push -u origin master`
+ 从远程仓库克隆时，Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。
  + 查看远程库信息：`git remote -v`


## Bug branch

+ 贮藏工作现场：`git stash`
+ 查看所有贮藏内容：`git stash list`
+ 恢复工作现场：
  + 恢复不删除stash内容：`git stash apply`
  + 恢复并删除stash内容：`git stash pop`
+ 复制一个特定的提交到当前分支以重放相同操作（修复bug）：`git cherry-pick <commit_id>`

## Log

+ commit日志：`git log`

## .gitignore

+ `.gitignore`也需要提交到版本库

```shell
# 通配符，排除所有.开头的隐藏文件：
.*
# 排除所有.class文件
*.class

# 例外规则
!.gitignore
!App.class
```
