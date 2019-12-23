# Git命令记录
## 常用配置
### 配置GitHub SSH key
点击头像 -- settings -- SSH and GPG keys -- New SSH key
使用命令，本地创建sshkey
`ssh-keygen -t rsa -C "GitHub邮箱"
复制生成的id_rsa.pub文件内容
### 配置命令行显示中文
`git config --global core.quotepath false`

## status
查看文件状态
git中的文件状态总的来说有两种：未跟踪(**untracked**)和已跟踪,
已跟踪的文件状态可能是:未修改，已修改(modified)，已暂存(stage)
```
# 以正常状态，显示文件状态
git status
# 以精简模式显示(untracked:??, add: A, modified: M)
git status -s
```

## add
将文件从未跟踪状态(**untracked**)添加到暂存区(**stage**)
```
git add xxx
```

## commit
将暂存区文件提交，文件状态由暂存切换到未修改
```
# 以指定注释提交暂存区文件
git commit -m "xxxxx"
# 提交文件，不用先添加到暂存区
git commit -a "xxx"
```

## branch
### 显示
```
# 显示本地分支(所有远程|所有本地和远程)
git branch [-r | -a]
```
### 创建
```
# 创建指定名称的分支
git branch xxx
```
### 删除
```
# 删除本地分支
git branch --delete xxx
```

## checkout
```
# 切换到指定分支
git checkout xxx
# 创建新分支并切换
git checkout -b xxx
```
## pull
## push