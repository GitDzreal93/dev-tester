# Git 使用

### Git的配置文件

**gitconfig**：对 git 行为进行配置

1. 系统配置：/etc/gitconfig
2. 全局配置：~/.gitconfig
3. 项目配置：.git/config

项目配置优先级高于全局配置

**.gitignore**：指定文件忽略规则

查看配置信息：`git config --list`

#### 配置个人信息

1. `git config user.email` 配置邮箱（当前项目）
2. `git config user.name` 配置用户名（当前项目）
3. `git config --global user.email` 配置邮箱（全局） 
4. `git config --global user.name` 配置用户名（全局）
5. `git config --system user.email` 配置邮箱（系统）
6. `git config --system user.name` 配置用户名（系统） 

#### 设置快捷键

修改 **.gitconfig** 文件

```shell
[alias]
        di = diff
        dis = diff --staged
        dic = diff HEAD^ HEAD
        dit = difftool --no-prompt --extcmd "icdiff"
        co = checkout
        br = branch
        ci = commit
        st = status
        pl = pull
        lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %C(bold blue)%s%Creset %Cgreen(%cr) <%an>%Creset' --abbrev-commit --date=relative
        lgs = log --stat --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %C(bold blue)%s%Creset %Cgreen(%cr) <%an>%Creset' --abbrev-commit --date=relative
        lga = log --all
        last = log -1
[icdiff]
        options = --highlight --line-numbers --no-bold
```

### 查看提交记录

**git log**

`git log` 查看过往提交记录

`git log —stat` 查看每次提交具体改动的文件

`git log -p` 查看文件的具体改动

`git log -a` 展示所有分支的改动

`git log -1` 查看最近1次提交

`git log -5` 查看最近5次提交

`git log —G patten` 查看改动内容包含patten的部分

**git diff**

`git diff` 查看工作区的变动

`git diff --staged` 查看暂存区的变动

`git diff HEAD^ HEAD` 查看最近一次提交的变动

**git grep**

`git grep --break --heading -n patten` 查看内容包含patten部分的改动

`git grep -L (line_begin,line_end):filepath patten` -L 表示行内查找

### 分支操作

`git branch` 显示所有本地分支，`*` 所在的位置，表示当前分支

`git branch new_branch` 创建新分支

`git checkout branch_name` 切换分支

`git checkout -b branch_name` 创建新分支并切换到新分支

`git checkout --track origin/feature` 远程检出新分支（远程和本地创建一个叫feature的分支）

`git checkout —- file_name`  拉取暂存区文件并将其替换成工作区文件

`git branch -d branch_name`  删除分支

`git branch -D branch_name` 强行删除分支 

`git tag tag_name` 打tag版本

`git tag -d tag_name` 删除tag

### 代码修改

**git stash**

`git stash` 储藏当前未提交的改动

`git stash -u` untracked 的文件也储藏

`git stash pop` 复原储藏（文件都复原成**未暂存**状态）

`git stash pop -—index` 复原储藏（已暂存的文件，还是恢复为暂存的状态）

**git reset**

 `git reset --soft`  让历史区与指定的提交保持一致，可以理解为撤销 `git commit`

 `git reset` or `git reset --mixed`  让暂存区和历史区与指定的提交保持一致，可以理解为撤销 `git add` 

`git reset --hard` 让工作区、暂存区和历史区都与指定的提交保持一致，可以理解为撤销所有改动，这是一个**不可挽回**的操作，请谨慎执行。

### 代码同步

**git fetch**

`git fetch` 将远程仓库的代码、 分支和 tag 都下载到本地。但不会改变本地代码

**git rebase** 和 **git merge**

[详细可参考：http://jartto.wang/2018/12/11/git-rebase/](http://jartto.wang/2018/12/11/git-rebase/)

`git rebase base_branch` 变基，改变某次提交的父提交（假设你的父分支是 base_branch）

`git merge base_branch` 把 base_branch 的代码合并到你当前的分支 

```shell
# 举个例子，你目前的开发分支是 branch_a，branch_a 是从上午10:30的master分支检出的
# 上午10:45，你的同事对master分支进行了提交

# 你有两种方式去拉取代码：
#（1）git rebase
(branch_a): git rebase master
# ==> 此时你的 branch_a 分支的代码的父提交变成了从上午10:45的提交中检出的，你在你的 branch_a 中 git log 将看不到你同事的提交记录，保持分支整洁

#（2）git merge
(branch_a): git merge master
# ==> 此时你的 branch_a 分支代码把最新的master代码合并到了你的代码中，你在你的 branch_a 中 git log 可以看到你同事的提交记录，但这样也会扰乱你的提交记录，让你看着很多很乱
```

**git rebase ** 还是 **git merge** ?：

1. 当需要在一个过时的分支（放置了很久）上面开发时，用 rebase 来同步 master 最新的分支改动
2. 合并当前分支的多次提交记录
3. 假如你的分支和别的同事共同开发，千万不要用`git rebase`，而是应该用`git merge`
4. 假如有冲突，`git merge` 仅需要解决一次冲突，`git rebase` 会出现大量不必要的冲突

**git pull**

`git pull branch_name` 从 branch_name 分支，拉取最新的提交并合并分支，等价于`git fetch + git merge`

**git commit** 

`git commit -m 提交信息` 将暂存区的代码提交到历史区

`git commit --all -m 提交信息` 等价于`git add . && git commit -m 提交信息 `

**git push**

`git push` 把本地历史区的代码提交到远程仓库分支

### 解决冲突

**在 rebase 或者 merge 之前，务必确保工作目录是干净的**

**查看文件冲突的方法**

```shell
#（1）
git diff --cc file

# ++<<<<<<<<<<< 0351231419023129..
# + 这里是当前文件改动
# ++===========
# + 这里是将要合入的改动
# ++>>>>>>>>>>> conflict_branch
```

**放弃解决冲突**

`git rebase --abort`

`git merge --abort`

**解决冲突**

```
# --ours:以我们为准， --theirs:以他们为准
git checkout --ours conflict_file
git add conflict_file
git commit
# 注意不用加 -m 选项，git 会默认生成一个 merge 的 message
```

**撤销合并**

`git reset --hard HEAD~` 回到上一次提交

**git reflog**

`git reflog`  git 命令进行操作的日志 