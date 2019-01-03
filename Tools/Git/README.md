<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*
# Git 常用命令清单

- [工程下载、分支的增删查改](#%E5%B7%A5%E7%A8%8B%E4%B8%8B%E8%BD%BD%E5%88%86%E6%94%AF%E7%9A%84%E5%A2%9E%E5%88%A0%E6%9F%A5%E6%94%B9)
  - [工程下载：](#%E5%B7%A5%E7%A8%8B%E4%B8%8B%E8%BD%BD)
  - [分支的增删查改](#%E5%88%86%E6%94%AF%E7%9A%84%E5%A2%9E%E5%88%A0%E6%9F%A5%E6%94%B9)
- [查看提交信息日志](#%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E4%BF%A1%E6%81%AF%E6%97%A5%E5%BF%97)
- [版本回退](#%E7%89%88%E6%9C%AC%E5%9B%9E%E9%80%80)
- [文件的添加、提交、拉取、推送、比对、合并](#%E6%96%87%E4%BB%B6%E7%9A%84%E6%B7%BB%E5%8A%A0%E6%8F%90%E4%BA%A4%E6%8B%89%E5%8F%96%E6%8E%A8%E9%80%81%E6%AF%94%E5%AF%B9%E5%90%88%E5%B9%B6)
- [常用选项和其他命令](#%E5%B8%B8%E7%94%A8%E9%80%89%E9%A1%B9%E5%92%8C%E5%85%B6%E4%BB%96%E5%91%BD%E4%BB%A4)
  - [git 中部分选项解释](#git-%E4%B8%AD%E9%83%A8%E5%88%86%E9%80%89%E9%A1%B9%E8%A7%A3%E9%87%8A)
  - [其他命令](#%E5%85%B6%E4%BB%96%E5%91%BD%E4%BB%A4)
- [配置相关](#%E9%85%8D%E7%BD%AE%E7%9B%B8%E5%85%B3)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 工程初始化及下载：

- 初始化本地仓库：git init
- clone 远程工程：git clone `https://XXXX/project.git`
- fetch 远程分支到本地某分支：git fetch origin `<orginname>`:`<localname>`

## 分支的增删查改

- 查看本地所有分支：git branch
- 查看远程所有分支：git branch -r
- 查看本地和远程所有分支：git branch -a
- 创建分支：git branch `<name>`
- 切换分支：git checkout `<name>`
- 创建并切换分支：git checkout -b `<name>` [origin/`<name>`]
- 把分支推送到远程：git push origin `<name>`
- 删除本地分支：git branch (-d | -D) `<name>`
- 删除远程分支：git push origin -d `<name>`
- 分支重命名：git branch (-m | -M) `<oldbranch>` `<newbranch>`

**注意：** 不能删除当前所在本地分支。

## 文件的添加、提交、拉取、推送、比对、合并

- 添加新增文件：git add README.md
- 添加所有新增文件：git add .
- 暂存变更文件：git stash [save "暂存备注"]
- 恢复暂存文件：git stash pop
- 提交变更文件：git commit -m "变更备注"
- 拉取远程代码：git pull [origin `<name>`]
- 推送到远程：git push origin `<name>`
- 比对两个本地分支：git diff `<name1>` `<name2>`
- 比对本地和远程分支：git diff `<name>` `origin/<name>`
- 比对两个分支变更的文件列表：git diff `<name1>` `<name2>` --stat
- 合并某分支到当前分支：git merge `<name>`
- 合并某次提交到当前分支：git cherry-pick `<commit id>`
- 撤销某次提交：git revert `<commit id>`
- 强制覆盖本地分支：  
    1、git fetch --all  
    2、git reset --hard origin/`<name>`   
    3、git pull  

## 查看提交日志和信息

- 查看分支最近一次的修改列表：git status
- 查看分支的日志信息(倒叙排列)
  - `git log`  查看commit id, Author, Date, commit info
  - `git shortlog` 按提交者分类显示提交信息
  - `git log --oneline` 只输出commit id 和 commit info
  - `git log --stat` 查看增删查改了哪些文件
- 查看分支的 commit 信息
  - `git show <commit id>` 查看某次提交的代码

## 标签

- 展示已有标签：git tag
- 展示过滤后的标签：git tag -l 'v1.4.2.*'
- 新建轻量级标签：git tag `<tagName>`
- 新建含附注的标签：git tag -a `<tagName>` -m `<备注信息>`
- 查看相应标签的版本信息：git show `<tabName>`
- 根据commit id 添加标签：git tag -a `<tagName>` `<commit id>`
- 推送到远程（分享标签）：git push origin `<tagName>`
- 推送本地所有新增标签：git push origin --tags

**注** -a: annotated

## 版本回退

- 回退到上一个版本：git reset --hard HEAD^
- 回退到上上个版本：git reset --hard HEAD^^
- 回退到上N个版本：git reset --hard HEAD~N
- 回退到某个版本：git reset --hard `<commit id>`
- 强制推送到远程分支：git push -f

## 变基

- git rebase 将一个feature分支变基到master分支  
master最新代码合并至feature分支：  
$ `git checkout feature`  
$ `git rebase master`  

**注意：**  
1、`HEAD` 指向的版本是当前版本，`^` 表示上一个版本，`~N` 表示上N个版本，`<commit id>`可简写  
2、`git log` 可以查看`<commit id>`  
3、`git reflog` 可以查看命令历史，用来回到某个未来的版本  

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

- 初始化git：`git init`
- 清空工程：`git rm -rf .`  
- 每隔X秒运行一次git pull：`for((i=1;i<=10000;i+=1)); do sleep X && git pull; done`
- 唤起git图形化工具：`gitk`
- 查看文件的每个部分是谁修改的：`git blame <fileName>`
    - 设置从第M行开始显示N行：`git blame -L M,+N <fileName>`

## 配置相关

- 查看当前配置： git config --list
- 修改 git 的 name 和 email ：
  - git config --global user.name `<name>`
  - git config --global user.email `<email>`
- 修改 git 的 push.default
    - git config --global push.default simple : 推送本地当前分支到远程（远程必须存在）
        - git push --set-upstream origin `<tagName>` : simple 通过此方法创建远程分支并推送
    - git config --global push.default current : 推送本地当前分支到远程（远程可以不存在）
- 设置常用别名：
  - git config --global alias.co checkout
  - git config --global alias.br branch
  - git config --global alias.ci commit
  - git config --global alias.st status

**提示：** Windows 用户可以把配置信息写入`.bat`文件
