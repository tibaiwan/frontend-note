# Git 常用命令清单

## 工程下载、分支的增删查改

### 工程下载：  
- clone 远程工程：git clone https://XXXX.git
- fetch 远程分支到本地某分支：git fetch origin `<orginname>`:`<localname>`

### 分支的增删查改
- 查看分支：git branch
- 查看远程所有分支：git branch -r
- 查看本地和远程所有分支：git branch -a
- 创建分支：git branch `<name>`
- 切换分支：git checkout `<name>`
- 创建并切换分支：git checkout -b `<name>`
- 合并某分支到当前分支：git merge `<name>`
- 把分支推送到远程：git push origin `<name>`
- 删除本地分支：git branch (-d | -D) `<name>`
- 删除远程分支：git push origin -d `<name>`
- 分支重命名：git branch (-m | -M) `<oldbranch>` `<newbranch>`

**注意：** 不能删除当前所在本地分支。

## 查看提交信息日志

- 查看分支最近一次的修改列表：git status
- 查看分支的commit信息(倒叙排列)
  - `git log`  查看commit id, Author, Date, commit info
  - `git shortlog` 按提交者分类显示提交信息
  - `git log --oneline` 只输出commit id 和 commit info
  - `git log --stat` 查看增删查改了哪些文件

## 版本回退

- 回退到上一版本：git reset --hard HEAD^
- 回退到上上版本：git reset --hard HEAD^^
- 回退到上上版本：git reset --hard HEAD~2
- 回退到某个版本：git reset --hard `<commit id>`
- 强制推送到远程分支：git push -f

**注意：**  
1、`HEAD` 指向的版本是当前版本，`^` 表示上一个版本，`~N` 表示上N个版本，`<commit id>`可简写  
2、`git log` 可以查看`<commit id>`  
3、`git reflog` 可以查看命令历史，用来回到某个未来的版本  

## 文件的添加、提交、拉取、推送、比对、合并

- 添加新增文件：git add README.md
- 添加所有新增文件：git add .
- 暂存变更文件：git stash [save "暂存备注"]
- 恢复暂存文件：git stash pop
- 提交变更文件：git commit -m "变更备注"
- 拉取远程代码：git pull [origin `<name>`]
- 推送到远程：git push origin `<name>`
- 比对两个分支：git diff `<name1>` `<name2>`
- 比对两个分支变更的文件列表：git diff `<name1>` `<name2>` --stat
- 比对本地和远程分支：git diff `<name>` `origin/<name>`
- 合并某个分支：git merge `<name>`
- 强制覆盖本地分支：  
    1、git fetch --all  
    2、git reset --hard origin/`<name>`   
    3、git pull  

## 常用选项和其他命令

### git 中部分选项解释
- `-f`  --force：强制
- `-d`  --delete：删除
- `-D`  --delete --force
- `-m`  --move：移动或重命名
- `-M`  --move --force
- `-r`  --remote：远程
- `-a`  --all：所有

### 其他命令
- 清空工程：  
$ `git rm -rf .`  
- 每隔X秒运行一次git pull：  
$ `for((i=1;i<=10000;i+=1)); do sleep X && git pull; done`
- 使用git rebase将一个feature分支变基到master分支  
$ `git checkout feature`  
$ `git rebase master`  

## 配置相关

- 查看当前配置： git config --list
- 修改 git 的 name 和 email ：
  - git config --global user.name `<name>`
  - git config --global user.email `<email>`

**提示：** Windows 用户可以把配置信息写入`.bat`文件
