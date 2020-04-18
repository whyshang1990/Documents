# Git命令记录

## 常用配置

### 配置GitHub SSH key

点击头像 -- settings -- SSH and GPG keys -- New SSH key
使用命令，本地创建sshkey
`ssh-keygen -t rsa -C "GitHub邮箱"
复制生成的id_rsa.pub文件内容

### 常用全局配置

```bash
# 查看配置信息
git config --list
# 配置命令行显示中文
git config --global core.quotepath false
# 在windows系统配置是否自动转换换行符
git config --global core.autocrlf false
# 设置git的界面编码
git config --global gui.encoding utf-8
# 设置 commit log 提交时使用 utf-8 编码
git config --global i18n.commitencoding utf-8
git config --global i18n.logoutputencoding utf-8
```

## 常用场景

***

### status

查看文件状态
git中的文件状态总的来说有两种：未跟踪(**untracked**)和已跟踪,
已跟踪的文件状态可能是:未修改，已修改(modified)，已暂存(staged)

```bash
# 以正常状态，显示文件状态
git status
# 以精简模式显示(untracked:??, add: A, modified: M)
git status -s
```

***

### add

将文件从未跟踪状态(**untracked**)添加到暂存区(**staged**)

```bash
git add xxx
```

### restore

```
# 将暂存区的文件取消暂存，即文件状态改变为未跟踪(untracked)
git restore --staged xxx
```

### commit

将暂存区文件提交，文件状态由暂存切换到未修改

```bash
# 以指定注释提交暂存区文件
git commit -m "xxxxx"
# 提交文件，不用先添加到暂存区
git commit -a "xxx"
```

***

### branch

```bash
# 显示本地分支(所有远程|所有本地和远程)
git branch [-r | -a]
# 创建指定名称的分支
git branch xxx
# 删除本地分支
git branch --d xxx
```

### checkout

```bash
# 切换到指定分支
git checkout xxx
# 创建新分支并切换
git checkout -b xxx
```

### merge

```bash
# 合并某name分支到当前分支
git merge <name>
```

***

### log

```bash
# 选项
# 提交记录在一行内展示
--oneline
# 展示前几条提交记录，X表示数量（1，2...）
-- nX
# 查看指定分支(远程，本地)
git log [origin/develop, develop]
```

### diff

比较当前本地和 Git 仓库，可用于简单修改后的仓库对比

```bash

```

***

### pull

**git pull <远程主机名> <远程分支名>:<本地分支名>**

```bash
# 拉取远程分支develop到本地develop
git pull origin develop:develop
# 拉取远程分支develop到当前激活的本地分支
git pull origin develop
# 拉取与当前激活本地方志存在追踪关系的远程分支
git pull origin
```

### push

**一般形式git push <远程主机名> <本地分支名> <远程分支名>**

```bash
# 表示将本地develop分支推送到与之存在追踪关系的远程分支（通常两者同名）
git push origin develop
# 省略本地分支，表示提交当前分支
git push origin
```

## Git 临时保存工作区

修复 bug 时，一般会通过创建新的 bug 分支进行修复，然后合并，最后删除。
当手头的 dev 分支的工作没有完成时，先把工作现场用命令 git stash 保存一下，然后切换回 master 分支，
创建一个新分支例如 bug-101 去修复 bug，然后将更改 merge 到 master 分支，
再回到 dev 分支，用 git stash pop 恢复之前的工作现场。
