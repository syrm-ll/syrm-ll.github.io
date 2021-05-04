# Git

```shell
# 使用master 替换暂存区，工作区不变
git reset HEAD

# 删除指定文件的暂存区缓存， 工作区不变
git rm --catch <file>


# !!! 使用暂存区替换所有工作区内容。 相当于撤回到之前的工作状态。
git checkout .
git checkout --<file>

# 使用HEAD指向的master分区中的全部或部分文件，替换工作区，清空暂存区。
git checkout HEAD .
git checkout HEAD <file>
```





# 基本使用

## 配置：git config

```shell
# git 的配置工具
git  config

# 显示当前所有配置信息
git config --list 

# 编辑配置文件： 当前仓库
git config -e

# 编辑配置文件：全局配置
git config -e --global

# 设置提交时候的信息 --global 意为用户级别 --system 系统级别， 去掉仅对当前项目生效
# 系统级配置文件: /etc/gitconfig
# 用户级别配置文件: ~/.gitconfig
# 项目级别配置文件: ./.git/config
git config --global user.name "runoob"
git config --global user.email test@runoob.com
# 设置默认的文本编辑器
git config --global core.editor vim
# 设置差异分析工具
git config --global merge.tool vimdiff

```



## 初始化从仓库

`git init [仓库名称]` 使用指定的仓库名称初始化git仓库， 如果没有指定名称， 默认使用 `.git` 文件夹

`git clone ` 从远程仓库复制一个项目

### 从远程仓库拉取

```shell
# repo: 仓库地址
# directory : 指定文件夹
git clone <repo> [directory]
```


## 提交和修改

```shell
# 添加文件到仓库
git add <file | .>
# 例: 追踪所有的.java 文件
git add *.java

# 显示仓库状态:有变化的文件
git status 

# 文件比较
git diff

# 提交到本地仓库(版本库)
git commit 

# 回退版本
git reset

# 删除工作区文件
git rm 

# 移动工作区文件
git mv 

```

### 真 · 基本操作

| 命令         | 说明                                     |
| :----------- | :--------------------------------------- |
| `git add`    | 添加文件到仓库                           |
| `git status` | 查看仓库当前的状态，显示有变更的文件。   |
| `git diff`   | 比较文件的不同，即暂存区和工作区的差异。 |
| `git commit` | 提交暂存区到本地仓库。                   |
| `git reset`  | 回退版本。                               |
| `git rm`     | 删除工作区文件。                         |
| `git mv`     | 移动或重命名工作区文件。                 |

## 提交日志

| 命令               | 说明                                 |
| :----------------- | :----------------------------------- |
| `git log`          | 查看历史提交记录                     |
| `git blame <file>` | 以列表形式查看指定文件的历史修改记录 |



## 远程操作

| 命令         | 说明               |
| :----------- | :----------------- |
| `git remote` | 远程仓库操作       |
| `git fetch`  | 从远程获取代码库   |
| `git pull`   | 下载远程代码并合并 |
| `git push`   | 上传远程代码并合并 |



```shell

```







> https://www.runoob.com/git/git-basic-operations.html



## 分支操作

```bash
git branch 				  # 显示所有本地分支
git branch -a			  # 显示所有分支
git branch --show-current # 显示当前分支
git branch <分支名>		# 创建分支

git branch -m <旧分支名> <新分支名>		# 重命名
git branch -d <分支名>					# 安全删除分支 (已经合并过的)
git branch -D <分支名>					# 强制删除分支
git checkout <分支名>					# 切换到指定分支


git merge <分支名>					# 合并分支名 默认fast-forward（快进）合并, --no-ff 不使用快进,生成一个新合并提交

git merge   						# 线性合并
```



## 标签操作

```bash
git tag					  # 查看当前版本的标签
git tag <标签名>			# 设置标签
git tag -a <标签名>		# 设置带注释的标签
git tag -d <标签名>		# 删除标签
```



